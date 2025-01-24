# cka-prep

* build/tag/push docker with name in the current directory with Dockerfile
$ docker build -t {imagename}.
$ docker tag {src} {destination}
$ docker push {dockerhubusername}/{dockerimage:tagname}

* To run with detach mode and destinated port
$ docker run -dp {3000:3000} {dockerimage}

* To getinto the container with interactive mode
$  docker exec -it {containername} sh

Multi-staged build in docker:

* AS installer --> 
creating a kind cluster:

 $ kind create cluster --image=kindest/node:v1.29.12@sha256:62c0672ba99a4afd7396512848d6fc382906b8f33349ae68fb1dbfe549f70dec --name cka-cluster1

yaml

$ kind get clusters
$ kind delete --name <cluster-name>
$ kind create cluster --name <cluster-name> --config <cluster-name>.yaml
 
* Three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker

kind create cluster --image=kindest/node:v1.29.12@sha256:62c0672ba99a4afd7396512848d6fc382906b8f33349ae68fb1dbfe549f70dec --name cka-cluster2 --config config.yaml

$ kubectl get nodes

* check the status of the master and worker nodes
$ kubectl config get-contexts
* change the context 
$ kubectl config use-context {my-cluster-name}

# kubernatis docs and kubernatis blogs are sub domain in the exam
* How to create pods
- Imperative
$ kubectl run pods

- declartive
 * json 
 * yaml 

 * Create a nginx pod through kubctl imperative way

$ kubctl run nginx-pod --image=nginx:latest
$ kubectl get pods
$ kubectl explain pods
$ kubectl create -f podman.yml
$ kubectl delete pod {name_of_the_pod}
$ kubectl apply -f pod.yaml
$ kubectl describe pod {name_of_the_pod}
$ kubectl edit pod {name_of_the_pod} ( no need to apply)
$ kubectl exec -it {nameOfThePod} -- sh  ( get in to the pod)
$ kubectl run nginx --image=nginx --dry-run=client -o yaml  ( to have the yaml)
$ kubectl get pods --show-labels ( will show the lables of the pods)
$ kubectl get pods -o wide ( extended information about the pods, like ip)
$ kubectl get node -o wide

## Day 08
* replication centre / replication set / deployment
$ kubectl get rc ( replication centre: legacy version)
$ kubctl get rs ( replication set: latest)

* To scale the replicas in imperative way
$ kubectl scale --replicas=10 rs/{replicaset-name}

$ kubectl get deploy (s/ReplicaSet/Deployment/g)
$ kubectl get all ( to get all the info)
$ kubectl set image deploy/nginx-deployment nginx=nginx:1.9.1 ( to set the image to a version)
$ kubectl rollout history deployment/nginx-deployment ( to check the deployment history)
$ kubectl rollout undo deployment/nginx-deployment ( to roll back to the last rev)

### imperative way deployment
$ kubectl create deploy deploy-nginx-new --image=nginx --dry-run=client -o yaml > imperative-deploy.yaml

## Day09 services
* 3 service types, clusterip, nodeport and load balancer
* service range of node port is from (default: 30000-32767)
$ kubectl explain service
$ kubectl explain svc
* default type is clusterIP
* loadbalancer keeps the port in the background, distributes the trafic among the ips
* Imperative way
$ kubectl expose rs {nameofthedeployment} --port=80 --target-port=8000


## Day10 Namespaces
$ kubectl get namespaces
$ kubectl get all --namespace=kube-system or kubectl get -n=kube-system
$ kubectl create ns {nameofthenamespace}
$ kubectl create deploy nginx-n-demo --image=nginx -n demo
$ kubectl get deploy -n demo
$ kubectl scale replicas=3 deploy/{nameofthedeployment} -n demo
$ kubectl scale replics={numberofreplicas}
$ kubectl expose deploy/{nameofthedeployment} --name=service-port --port -n=demo 
* find the domain name from /etc/resolve.conf and then do the curl for service name
* Using fully qualified domain name

## Multi-container pods
* can have multiple init containers
$ kubectl apply -f pod-multicontainer.yaml

* create deploy and expose nginx
$ kubectl create deploy nginx-deploy --image nginx --port 80
$ kubectl exposedeploy nginx-deploy --name myservice --port 80

* create deploy and expose reddit
$ kubectl create deploy mydb --image redis --port 80
$ kubectl expose deploy mydb --name mydb --port 80

## Daemonset 
* Daemonset runs on all the available nodes except the control panel node
$ kubectl get ds -A
* 5 syntax in a cron job * * * * * --> mins hours dayofthemonth month dayofweek
* Apart from cron job there is also job which is used when trying to complete the task once


