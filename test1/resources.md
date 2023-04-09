## 资源清单
新建一个 `pod.yaml`
```pod
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
    - name: app
      image: nginx:latest
      imagePullPolicy: IfNotPresent
```
使用命令允许：
```
kubectl create -f pod.yaml
```
查看pod的情况：
```
@ThinkingXuan ➜ /workspaces/KubernetersLearning/test1 (main) $ kubectl get pod
NAME        READY   STATUS    RESTARTS   AGE
myapp-pod   1/1     Running   0          24m
```
查看pod的具体情况：
```
@ThinkingXuan ➜ /workspaces/KubernetersLearning/test1 (main) $ kubectl get pod -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
myapp-pod   1/1     Running   0          25m   10.244.0.4   minikube   <none>           <none>
```

我们可以看到pod已经启动成功，并且IP为10.244.0.4，但是这个pod的ip是无法直接访问的，因为实际pod的执行是在kicbase容器中，所以我们先使用docker exec进入这个容器，才可以访问。
```
root@minikube:/# curl 10.244.0.4
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```