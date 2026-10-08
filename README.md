# <div align="center">$${\color{purple}Kubernetes  \space Basic \space Testings}$$

<br>

- #### Start your Minikube cluster:
  ```
  minikube star
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

  Create deployment agnhost:2.53:
  ```
  kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53 -- /agnhost netexec --http-port=8080
 
  kubectl get pod,service,deployment -o wide   #verifying
  ```
  Expose the application as a Service:  
  To access the container's web server from outside the Minikube cluster, expose the deployment as a LoadBalancer type service:
  ```
  kubectl expose deployment hello-node --type=LoadBalancer --port=8080
  ```

  ```
  kubectl get all
  kubectl exec -it <pod-name> -- bash          #go inside agnhost:2.53 image os
  
  kubectl exec <pod-name> -- /agnhost --help   #usage: list all subcomand
  kubectl exec <pod-name> -- /agnhost dns-suffix
  kubectl exec <pod-name> -- /agnhost dns-server-list
  
  ```
