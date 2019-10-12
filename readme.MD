
# setup local env
https://minikube.sigs.k8s.io/docs/

## install virtualbox
https://www.virtualbox.org/
## download minikube
https://github.com/kubernetes/minikube/releases
## start minikube
```
minikube start \
    --cpus 4 \
    --memory 4096 \
    --network-plugin=cni \
    --enable-default-cni \
    --bootstrapper=kubeadm \
    --kubernetes-version v1.15.4 \
    --image-mirror-country=cn \
    --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers
```
# microsoft demo, run nginx as webserver
```
kubectl run --image=nginx nginx
```
# show running pod
```
kubectl get po --show-labels -owide -w
```
# expose svc
```
kubectl expose deploy nginx --selector run=nginx --port=80 --type=NodePort
```
# check svc detail
```
kubectl get svc
```
# check nodeip
```
minikube ssh
ifconfig eth1
```
# access service
```
curl <nodeip>:<nodeport>
```
# run envoy
```
kubectl create configmap envoy-config --from-file=envoy.yaml
kubectl create -f envoy-deploy.yaml
kubectl expose deploy envoy --selector run=envoy --port=10000 --type=NodePort
```
# access service
```
curl <nodeip>:<nodeport>
```
# scale up/down/failover
```
cat game.properties
```
# configmap from file
```
kubectl create configmap game-config --from-file=game.properties
kubectl create configmap game-env-config --from-env-file=game.properties
kubectl get configmap -oyaml game-config
```
# configmap from literal
```
kubectl create configmap special-config --from-literal=special.how=very --from-literal=special.type=charm
```
# downward api pod
```
kubectl create -f downward-api-pod.yaml
kubectl get po downward-api-pod
kubectl logs -f downward-api-pod
```
# volume
```
kubectl create -f configmap-volume-pod.yaml
kubectl get po
kubectl logs -f configmap-volume-pod
```
# readiness problem
```
kubectl create -f centos-readiness.yaml
```
# multiple container pods
```
kubectl create -f multiple-container-pod.yaml
```
# get customized columns
```
kubectl get svc  -o=custom-columns=NAME:.metadata.name,CREATED:'.metadata.annotations'
```

# install helm 3 (must be 3 which does not need tiller)
https://github.com/helm/helm/releases
# install istio
```
git clone https://github.com/istio/istio.git
cd istio
```
```
vi install/kubernetes/helm/istio-init/values.yaml
vi install/kubernetes/helm/istio/values.yaml
```
set
```
hub: docker.io/istio
tag: 1.3.3
```
```
kubectl create ns istio-system
helm install istio-init install/kubernetes/helm/istio-init  --namespace istio-system
helm install istio install/kubernetes/helm/istio  --namespace istio-system
```
# clean up
```
kubectl delete clusterrolebinding istio-init-admin-role-binding-istio-system
kubectl delete clusterrole istio-init-istio-system
```