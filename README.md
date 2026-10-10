# <div align="center">$${\color{purple}Kubernetes  \space Basic \space Testings}$$

<br>

- #### Start your Minikube cluster:
  ```
  minikube star --driver=docker
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

- #### Agnhost Usage:
  ```python
  $ kubectl run test-client --image=registry.k8s.io/e2e-test-images/agnhost:2.53
  $ kubectl exec pod/test-client  -- /agnhost  --help
  Usage:
  app [command]

  Available Commands:
  audit-proxy                           Listens on port 8080 for incoming audit events
  completion                            Generate the autocompletion script for the specified shell
  connect                               Attempts a TCP, UDP or SCTP connection and returns useful errors
  crd-conversion-webhook                Starts HTTP server on port 443 for testing CustomResourceConversionWebhook
  dns-server-list                       Prints the host's DNS Server list
  dns-suffix                            Prints the host's DNS suffix list
  entrypoint-tester                     Prints the args it's passed and exits
  etc-hosts                             Prints the host's /etc/hosts file
  fake-gitserver                        Fakes a git server
  grpc-health-checking                  Starts a simple grpc health checking endpoint
  guestbook                             Creates a HTTP server with various endpoints representing a guestbook app
  help                                  Help about any command
  inclusterclient                       Periodically poll the Kubernetes "/healthz" endpoint
  liveness                              Starts a server that is alive for 10 seconds
  logs-generator                        Outputs lines of logs to stdout uniformly
  mounttest                             Creates files with given permissions and outputs FS type, owner, mode, permissions, contents of files
  net                                   Creates webserver or runner for various networking tests
  netexec                               Creates HTTP(S), UDP, and (optionally) SCTP servers with various endpoints
  nettest                               Starts a tiny web server for checking networking connectivity
  no-snat-test                          Creates the /checknosnat and /whoami endpoints
  no-snat-test-proxy                    Creates a proxy for the /checknosnat endpoint
  pause                                 Pauses the execution
  port-forward-tester                   Creates a TCP server that sends chunks of data
  porter                                Serves requested data on ports specified in ENV variables
  resource-consumer-controller          Starts a HTTP server that spreads requests around resource consumers
  serve-hostname                        Serves the hostname
  stress                                Lightweight compute resource stress utlity
  tcp-reset                             Serves on a tcp port and RST the connections received
  test-service-account-issuer-discovery Tests the ServiceAccountIssuerDiscovery feature
  test-webserver                        Starts a simple HTTP fileserver
  webhook                               Starts a HTTP server, useful for testing MutatingAdmissionWebhook and ValidatingAdmissionWebhook

  Flags:
  -h, --help                           help for app
      --log-flush-frequency duration   Maximum number of seconds between log flushes (default 5s)
  -v, --v Level                        number for the log level verbosity
      --version                        version for app
      --vmodule moduleSpec             comma-separated list of pattern=N settings for file-filtered logging (only works for the default text log format)

  Use "app [command] --help" for more information about a command.
  ```

- **connect:** Attempts a TCP, UDP or SCTP connection and returns useful errors  
  - Create agnhost-backend pod and service, the target server
  - Create hello-node pod, the testing server
  ```
  kubectl run agnhost-backend --image=registry.k8s.io/e2e-test-images/agnhost:2.53 -- netexec --http-port=1111
  kubectl expose  pod/agnhost-backend --port=2222 --target-port=1111 --protocol=TCP
  kubectl run hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53
  ```
  - List all pods and serices
  ```python
  $ kubectl get all
  NAME                  READY   STATUS    RESTARTS   AGE
  pod/agnhost-backend   1/1     Running   0          14s
  pod/hello-node        1/1     Running   0          13s

  NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
  service/agnhost-backend   ClusterIP   10.111.10.160   <none>        2222/TCP   13s
  ```
  - Testing `connect` command
  ```
  kubectl exec pod/hello-node -- /agnhost connect agnhost-backend:2222 --protocol=tcp
  ```
<br>

- **audit-proxy:** Listens on port 8080 for incoming audit events  
  Create and run the audit-proxy subcommand inside a Kubernetes Pod
  ```
  kubectl run test-agnhost --image=registry.k8s.io/e2e-test-images/agnhost:2.53 --port=8080 -- audit-proxy
  kubectl port-forward pod/agnhost-audit-proxy 8080:8080
  ```
  Inspect the proxy logs:
  ```
  kubectl logs -f test-agnhost
  ```
  Trigger events:
  ```

  ```
<br>

- **completion:** Generate the autocompletion script for the specified shell







  
  ```
  kubectl run  hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53 --port=8080 -- netexec
  kubectl run  hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53 -- netexec --http-port=8080
  kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53
  kubectl expose deployment hello-node --type=LoadBalancer --port=8080

  kubectl create deployment hiep-test --image=registry.k8s.io/e2e-test-images/agnhost:2.53 --port=8080 --replicas=2  -- /agnhost netexec --http-port=8080
  kubectl run agnhost-backend --image=registry.k8s.io/e2e-test-images/agnhost:2.53 --port=8080 -- netexec --http-port=8080
  kubectl expose  pod/agnhost-backend --port=8080 --target-port=8080
  ```
  ```python

  ```

  ```
  kubectl get all
  kubectl exec -it <pod-name> -- bash          #go inside agnhost:2.53 image os
  
  kubectl exec <pod-name> -- /agnhost --help   #usage: list all subcomand
  kubectl exec <pod-name> -- /agnhost dns-suffix
  kubectl exec <pod-name> -- /agnhost dns-server-list
  
  ```
  ```
  kubectl create deployment my-nginx --image=nginx
  kubectl expose deployment my-nginx --port=80 --target-port=8080
  kubectl exec hello-node-78b9f44d96-qszq7 -- /agnhost connect  10.244.0.25:80
  ```





  
