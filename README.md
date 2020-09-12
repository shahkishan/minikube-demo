# Spring Boot with Minikube
This is a sample project for running Spring Boot Application with Minikube

Follow the below steps to run this application inside a minikube container.

* Clone the repository with the following command: 
```sh 
$ git clone https://github.com/shahkishan/minikube-demo.git
```
* Go to the project root folder inside the terminal and create a jar file using the following command:
```sh
$ ./gradlew clean build
```
* Install and run minikube, if not already installed. Follow the guide given in 
https://kubernetes.io/docs/tasks/tools/install-minikube/
* Set minikube to use local docker registry by setting envirnonment variables with eval: 
```sh
$ eval $(minikube docker-env)
```
* Create docker image of the project using:
```sh
$ docker image build . -t minikube-demo
```
* Create Kubernetes deployment to run the service: 
```sh
$ kubectl create deployment minikube-demo --image=minikube-demo:latest -o yaml > deployment.yaml
```
* Open `deployment.yaml` file and change `imagePullPolicy` to `IfNotPresent`
* Delete created deployment: 
```sh
$ kubectl delete deployment minikube-demo
```
* Apply edited configurations:
```sh
$ kubectl apply -f deployment.yaml
```  
* Create kubernetes service which can access the created pod: 
```sh
$  kubectl expose deployment minikube-demo --type=NodePort --port=8080 -o yaml > service.yaml
```
* Get public ip to the service to access it locally: 
```sh
$  minikube service minikube-demo
```
* Copy the url from terminal and use it to access your service
* Access Minikube dashboard: 
```sh
$ minikube dashboard
```