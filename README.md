# KubernetersLearning

### 1. 环境配置

本文件通过使用Github的codeSpace功能来作为K8S的服务器。优点非常明显，不要翻墙就可以下载所有镜像，专注于功能的学习，服务器免费使用，没有关机丢失数据的问题。缺点就是没有体验折腾的过程，可能理解没有那么深刻。

由于只有一个服务器可以使用，所以使用minikube工具来模拟K8S的使用。当我们新建codeSpace之后，检查下环境:
```shell
docker version
kubectl version
minikube version
```
发现都已经安装了，现在开始学习K8S的吧。

启动minikube:
```
minikube start
```
稍等片刻后，minikube启动完成，我们通过`docker ps`查看。发现minikubet运行在一个叫做kicbase的容器当中：
```shell
@ThinkingXuan ➜ /workspaces/KubernetersLearning (main) $ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS                                                                                                                                  NAMES
b493fe1c301a   gcr.io/k8s-minikube/kicbase:v0.0.37   "/usr/local/bin/entr…"   8 minutes ago   Up 8 minutes   127.0.0.1:32772->22/tcp, 127.0.0.1:32771->2376/tcp, 127.0.0.1:32770->5000/tcp, 127.0.0.1:32769->8443/tcp, 127.0.0.1:32768->32443/tcp   minikube
```
通过dokcer exec来查看连接这个容器。
```
docker exec -it b493fe1c301a /bin/bash
```
这个容器里面也部署了一个docker引擎，然后用docker来装载K8S的环境。
```
root@minikube:/# docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS          PORTS     NAMES
409ad3a8dd91   6e38f40d628d                "/storage-provisioner"   14 minutes ago   Up 14 minutes             k8s_storage-provisioner_storage-provisioner_kube-system_672cbc42-6122-49eb-83bf-2b1335fe4eb5_1
3cc2cad1a91d   5185b96f0bec                "/coredns -conf /etc…"   15 minutes ago   Up 15 minutes             k8s_coredns_coredns-787d4945fb-w94zf_kube-system_7c846aa8-99ac-4234-a221-f21b42191098_0
f411e90ff39e   46a6bb3c77ce                "/usr/local/bin/kube…"   15 minutes ago   Up 15 minutes             k8s_kube-proxy_kube-proxy-89rdj_kube-system_b15e9c02-192c-4111-aa2c-24383daa3a9b_0
1a64621e8ea4   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_storage-provisioner_kube-system_672cbc42-6122-49eb-83bf-2b1335fe4eb5_0
c397281dedc5   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_coredns-787d4945fb-w94zf_kube-system_7c846aa8-99ac-4234-a221-f21b42191098_0
b294a0e48944   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_kube-proxy-89rdj_kube-system_b15e9c02-192c-4111-aa2c-24383daa3a9b_0
a7efdfa4596c   fce326961ae2                "etcd --advertise-cl…"   15 minutes ago   Up 15 minutes             k8s_etcd_etcd-minikube_kube-system_a121e106627e5c6efa9ba48006cc43bf_0
6b87b0e1ea6e   e9c08e11b07f                "kube-controller-man…"   15 minutes ago   Up 15 minutes             k8s_kube-controller-manager_kube-controller-manager-minikube_kube-system_5175bba984ed52052d891b5a45b584b6_0
1774ac3e11da   655493523f60                "kube-scheduler --au…"   15 minutes ago   Up 15 minutes             k8s_kube-scheduler_kube-scheduler-minikube_kube-system_197cd0de602d7cb722d0bd2daf878121_0
586b374fcab3   deb04688c4a3                "kube-apiserver --ad…"   15 minutes ago   Up 15 minutes             k8s_kube-apiserver_kube-apiserver-minikube_kube-system_5239bb256c1be9f71fd10c884d9299b1_0
c7df4f2234fd   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_kube-apiserver-minikube_kube-system_5239bb256c1be9f71fd10c884d9299b1_0
1613f461b894   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_kube-scheduler-minikube_kube-system_197cd0de602d7cb722d0bd2daf878121_0
281df890f1cb   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_etcd-minikube_kube-system_a121e106627e5c6efa9ba48006cc43bf_0
1341747d96cb   registry.k8s.io/pause:3.6   "/pause"                 15 minutes ago   Up 15 minutes             k8s_POD_kube-controller-manager-minikube_kube-system_5175bba984ed52052d891b5a45b584b6_0
```