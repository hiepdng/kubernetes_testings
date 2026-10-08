# <div align="center">$${\color{purple}Kubernetes  \space Basic \space Testings}$$

<br>

- #### Start your Minikube cluster:
  ```
  minikube star
  ```
  ```
  kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53 -- /agnhost netexec --http-port=8080
  ```
  Checking minikube cluster info.:
  ```
  minikube status
  kubectl get nodes
  kubectl cluster-info
  kubectl get pods -A
  minikube addons list

  #get all api-resources
  for i in `kubectl api-resources | awk '{print $1}'`; do echo  -e "-----------------\nkubectl get $i\n" && kubectl get $i; done
  ```
