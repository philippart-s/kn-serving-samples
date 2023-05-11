## Hello world from Quarkus demonstration

### Quarkus application (Hello, World!)
 - repository of the Quarkus application: https://github.com/philippart-s/hello-world-from-quarkus
 - main class: [GreetingResource.java](https://github.com/philippart-s/hello-world-from-quarkus/blob/main/src/main/java/fr/wilda/GreetingResource.java)
 - Dockerfile: [Dockerfile.jvm](https://github.com/philippart-s/hello-world-from-quarkus/blob/main/src/main/docker/Dockerfile.jvm)

### Deploy the application with kubectl
 - create a [service.yaml](./service.yaml) file
 - create a namespace: `kubectl create ns knative-samples`
 - apply the CR: `kubectl apply -f ./hello-world/service.yaml`
 - get the URL: `kubectl get ksvc helloworld-from-quarkus  --output=custom-columns=NAME:.metadata.name,URL:.status.url -n knative-samples`
 - test the application: `curl http://helloworld-from-quarkus.knative-samples.162.19.64.180.sslip.io/hello`
 - display the pods: `kubectl get pods -n knative-samples -w`
 - wait 30s and display again the pods
 - delete the application : `kubectl delete -f ./hello-world/service.yaml`

### Deploy the application with kn
 - create a namespace: `kubectl create ns knative-samples`
 - create a service: `kn service create helloworld-from-quarkus --env COLOR="blue" --image wilda/hello-world-from-quarkus:1.2.0 -n knative-samples`
 - get the URL: `kn service describe helloworld-from-quarkus -o url -n knative-samples`
 - test the application: `curl http://helloworld-from-quarkus.knative-samples.162.19.64.180.sslip.io/hello`

### Canary deployment with Knative
 - list the current revision of the application: `kn revisions list -n knative-samples`
 - deploy a new version of the application: `kn service update helloworld-from-quarkus -n knative-samples --env COLOR="green"`
 - split the traffic between the two revisions: `kn service update helloworld-from-quarkus --traffic helloworld-from-quarkus-00001=50 --traffic @latest=50 -n knative-samples`
 - display the conf: `kn revisions list -n knative-samples`
 - test the application (twice): `curl http://helloworld-from-quarkus.knative-samples.162.19.64.180.sslip.io/hello`
 - delete the service: `kn service delete helloworld-from-quarkus -n knative-samples`