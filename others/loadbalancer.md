## 516030910273
# Load balance􏰃􏰋 􏰀􏰃􏰝􏰃􏰚􏰄􏰜􏰃􏰋 􏰀􏰃
## Load balance􏰃􏰋 in kubernetes􏰀􏰃􏰝􏰃􏰚􏰄􏰃􏰋 􏰀􏰃
### Pod
* 每一个Pod都有一个独立的IP地址，甚至是同一个节点上的Pod，可以看出Pod的IP是动态的，它随Pod的创建而创建，随Pod的销毁而消失，这就引出一个问题：如果由一组Pods组合而成的集群来提供服务，那如何访问这些Pods呢？
### Service
* 一个Service可以看作一组提供相同服务的Pods的对外访问接口，Service作用于哪些Pods是通过label selector来定义的，这些Pods能被Service访问
* Service有四种type: ClusterIP(默认）、NodePort、LoadBalancer、ExternalName. 其中NodePort和LoadBalancer两类型的Services可以对外提供服务
### create loadbalancer services(v1)
before
```
$ kubectl get services
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
front        NodePort    10.110.87.31    <none>        8080:32145/TCP   3d3h
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          5d9h
tomcat       NodePort    10.98.208.200   <none>        8080:32143/TCP   5d4h
```
```
$ kubectl edit services front -o yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-01-04T07:24:22Z"
  labels:
    name: front
  name: front
  namespace: default
  resourceVersion: "706020"
  selfLink: /api/v1/namespaces/default/services/front
  uid: c189c641-0ff1-11e9-8aad-525400ee727d
spec:
  clusterIP: 10.110.87.31
  externalTrafficPolicy: Cluster
  ports:
  - nodePort: 32145
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: front
  sessionAffinity: None
  type: LoadBalancer    #used to be NodePort 
status:
  loadBalancer: {}
```
after
```
~$ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
front        LoadBalancer   10.110.87.31    <pending>     8080:32145/TCP   3d4h
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          5d10h
tomcat       NodePort       10.98.208.200   <none>        8080:32143/TCP   5d5h
```
now, <pending>
```
status:
  loadBalancer: {}
```
do

```
kubectl create -f ng.yaml 
```
```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: nginx
spec:
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
        image: nginx:1
        ports:
        - name: http
          containerPort: 80
```
then
```
$ kubectl get svc
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
front        LoadBalancer   10.110.87.31    8.0.0.0       8080:32145/TCP   4d8h
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          6d13h
tomcat       LoadBalancer   10.98.208.200   8.0.0.1       8080:32143/TCP   6d8h
```
```
loadBalancer: 
    Ingress: 8.0.0.0
```