# kubernete cluster搭建说明

## 服务器信息
**hostname                      公网IP           内网IP 备注**  
app     212.64.60.120	172.17.0.8  kubernete（工作节点） tomcat 前端/后端  
name	212.64.52.235	172.17.0.12 kubernete（工作节点） tomcat 前端/后端  
data1	212.64.66.254	172.17.0.15 kubernete（工作节点） tomcat 前端/后端 mysql 数据库  
master  111.231.137.229	172.17.32.10    kubernete（单一master节点）  

ubuntu@master:~$ kubectl get pods -o wide  
**NAME                      READY   STATUS    RESTARTS   AGE    IP             NODE    NOMINATED NODE   READINESS GATES**  
front-789588d94-4rkx2     1/1     Running   0          2d5h   192.168.1.14   app     <none>           <none>  
front-789588d94-gsw82     1/1     Running   0          2d5h   192.168.2.25   name    <none>           <none>  
tomcat-8597648b67-8vm2n   1/1     Running   0          4d5h   192.168.1.4    app     <none>           <none>  
tomcat-8597648b67-d76wz   1/1     Running   0          4d5h   192.168.3.8    data1   <none>           <none>  

ubuntu@master:~$ kubectl get nodes  
**NAME     STATUS   ROLES    AGE     VERSION**  
app      Ready    <none>   4d10h   v1.13.0  
data1    Ready    <none>   4d10h   v1.13.1  
master   Ready    master   4d10h   v1.13.1  
name     Ready    <none>   4d10h   v1.13.1  

ubuntu@master:~$ kubectl get service  
**NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE**  
front        NodePort    10.110.87.31    <none>        8080:32145/TCP   2d5h  
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          4d10h  
tomcat       NodePort    10.98.208.200   <none>        8080:32143/TCP   4d5h  


## 对外端口
**前端：**
111.231.137.229:32145  
**后端：**
111.231.137.229:32143  
**数据库：**
212.64.66.254:3306  

![avatar](https://raw.githubusercontent.com/black197/Introduction-to-Large-scale-cluster-management-system/milky/pics/QQ%E5%9B%BE%E7%89%8720190106202918.png)  

![avatar](https://raw.githubusercontent.com/black197/Introduction-to-Large-scale-cluster-management-system/milky/pics/QQ%E5%9B%BE%E7%89%8720190106202916.png)  

## Docker Hub镜像
**前端：**
edithwu/kubernetes:ebook_back  
**后端：**
edithwu/kubernetes:latest

## YAML
**前端：**
见deployment_front、service_front.yaml  
**后端：**
见deployment_back、service_back.yaml  

## 参考教程
http://www.imooc.com/article/266932  
