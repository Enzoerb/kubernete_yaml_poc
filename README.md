# Kubernetes Yaml PoC

In this PoC I will be covering mainly how to create and manage yml file for k8s.

we will use the yaml file to create kubernetes resources like pods, deployments and services.

At First we need to pay attention to the "kind" key, it is responsible to specify which element you're going to create or edit.
When talking about kinds we can have things like:

Pods, Secrets, Service, Deployment and other resources

important fields are:
| field | description |
| ----------- | ----------- |
| replicas | tells how many of those you will want |
| spec | object that will contain some specifications about certain resource |
| maxSurge | tells us how many pods we can go up then the required number of pods |
| maxUnavailable | tells us how much “under” can we go then the required number of pods |
| minReadySeconds | specifies the minimum number of seconds for which a newly created Pod should be ready without any of its containers crashing |
| revisionHistoryLimit | specify how many old ReplicaSets for this Deployment you want to retain |
| labels | intended to be used to specify identifying attributes of objects |
| selector | using it you can identify a set of objects |
| matchLabels | it is a map of {key,value} pairs used within a selector

## Examples

now, let's take a look at some examples

### Example 1

the first one was taken from: [kubernetes docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

here you can see a yaml that will specify a creation of a Deployment, using the apiVersion "apps/v1".
You can also see that a name and app label are specified in it's metadata, in the spec the yaml asks for 3 of those to be created in "replicas", matching the app label "nginx" and then specifying the name, image and port of the container that we want to create.

### Example 2

the second one was taken from: [kubernetes docs](https://kubernetes.io/docs/concepts/services-networking/service/#defining-a-service)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
  clusterIP: 10.0.171.239
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - ip: 192.0.2.127
```

 here you can see that we are creating a Service, using the apiVersion "v1", it will be called "my-service".
 Then the selector identifying "MyApp", end we are running the service as TCP using the port 80 and targeting the port 9376, we're' also specifying the clusterIP and that the service will actually be a load balancer.