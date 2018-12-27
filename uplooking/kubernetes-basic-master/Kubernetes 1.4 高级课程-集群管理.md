# Kubernetes 1.4 高级课程

###### kissingwolf@gmail.com

[TOC]

## Kubernetes 集群管理

### Node 节点管理

在前面基础课程中，我们介绍了Kubernetes 集群是由两种基础设备组成的：Master和Node。Master负责管理和维护Kubernetes集群信息，并向Node下放任务和接收反馈信息。Node负责集群负载，在Node上真实的运行Kubernetes Pod 以及容器实例。

随着Kubernetes集群的构建，Node节点数量会不断增加，Node节点由于其自身的故障或网络原因也会出现离线或剔除。我们就需要对其做相应的操作。

#### Node节点的隔离和恢复

在硬件或网络维护的时候，我们需要将某些Node 节点进行隔离，使其脱离Kubernetes 集群的调度。Node 节点脱离Kubernetes集群调度的目的是为了避免Master 再为其分配Pod 运行，原有运行在其上的Pod 并不会自动迁移到其它没有脱离Kubernetes 集群的Node节点上。如果希望Node 节点离线，并快速将脱离Kubernetes 调度的Node 节点上Pod 转移，需要手工完成操作。

隔离和恢复Node 节点有两种方法，使用配置文件或手工执行命令。

首先我们查看当前的Kubernetes 集群节点状态：

```shell
[root@master0 ~]# kubectl  get node
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     15d
```

你看到结果应该和上面类似，masterN 是Kubernetes 集群的Master ，nodeaN 和 nodebN 是Kubernetes 集群中的Node 节点。整个环境是在基础课程中就安装完成的标准环境，如果你的环境有问题，请在你的物理机上修改并运行init_k8s.sh，初始化Kubernetes试验环境。

使用配置文件方法，首先需要创建配置文件unsheduleable_node.yaml，内容如下：

```yaml
apiVersion: v1  # 配置版本
kind: Node # 配置类型
metadata: # 元数据
  name: nodeb0.example.com # 配置文件元数据名
  labels: # 标签名
    kubernetes.io/hostname: nodeb0.example.com # 需要隔离的 Node 节点
spec:
  unschedulable: true # spec.unschedulable 设置为true 时，Node 节点被隔离，
                      # 设置为false时，Node 节点恢复。 默认值为false
```

在默认情况下，nodebN.example.com 节点的状态和信息如下：

```shell
[root@master0 ~]# kubectl describe node nodeb0.example.com
Name:			nodeb0.example.com
Labels:			beta.kubernetes.io/arch=amd64
			beta.kubernetes.io/os=linux
			kubernetes.io/hostname=nodeb0.example.com 
Taints:			<none>
CreationTimestamp:	Tue, 22 Nov 2016 12:49:20 +0800
Phase:
Conditions:
  Type			Status	LastHeartbeatTime			LastTransitionTime			Reason				Message
  ----			------	-----------------			------------------			------				-------
  OutOfDisk 		False 	Wed, 07 Dec 2016 14:05:44 +0800 	Wed, 23 Nov 2016 16:36:20 +0800 	KubeletHasSufficientDisk 	kubelet has sufficient disk space available
  MemoryPressure 	False 	Wed, 07 Dec 2016 14:05:44 +0800 	Tue, 22 Nov 2016 12:49:19 +0800 	KubeletHasSufficientMemory 	kubelet has sufficient memory available
  DiskPressure 		False 	Wed, 07 Dec 2016 14:05:44 +0800 	Tue, 22 Nov 2016 12:49:19 +0800 	KubeletHasNoDiskPressure 	kubelet has no disk pressure
  Ready 		True 	Wed, 07 Dec 2016 14:05:44 +0800 	Wed, 23 Nov 2016 16:36:20 +0800 	KubeletReady 			kubelet is posting ready status
Addresses:		172.25.0.12,172.25.0.12
Capacity:
 alpha.kubernetes.io/nvidia-gpu:	0
 cpu:					2
 memory:				1016792Ki
 pods:					110
Allocatable:
 alpha.kubernetes.io/nvidia-gpu:	0
 cpu:					2
 memory:				1016792Ki
 pods:					110
System Info:
 Machine ID:			ecd4a28875734de9bf2cb5e40cbf88da
 System UUID:			C7207044-587A-4F66-9AF7-7E7262AD9DA9
 Boot ID:			3ddb17ed-0d77-4e17-91d9-b36301640f11
 Kernel Version:		3.10.0-327.el7.x86_64
 OS Image:			Red Hat Enterprise Linux Server 7.2 (Maipo)
 Operating System:		linux
 Architecture:			amd64
 Container Runtime Version:	docker://1.12.2
 Kubelet Version:		v1.4.0
 Kube-Proxy Version:		v1.4.0
ExternalID:			nodeb0.example.com
Non-terminated Pods:		(4 in total)
  Namespace			Name						CPU Requests	CPU Limits	Memory Requests	Memory Limits
  ---------			----						------------	----------	---------------	-------------
  kube-system			kube-proxy-amd64-gzmv5				0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			monitoring-grafana-927606581-45lpl		0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			monitoring-influxdb-3276295126-ec2nf		0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			weave-net-cfenz					20m (1%)	0 (0%)		0 (0%)		0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.
  CPU Requests	CPU Limits	Memory Requests	Memory Limits
  ------------	----------	---------------	-------------
  20m (1%)	0 (0%)		0 (0%)		0 (0%)
```

请保证你的配置信息与实际nodebN.example.com 节点的信息信息中Name和Lables显示相关项目一致。

通过kubectl replace 命令修改Node 状态：

```shell
[root@master0 ~]# kubectl replace -f unsheduleable_node.yaml
node "nodeb0.example.com" replaced
```

随后查看Node 状态，可以看到Node 节点 nodebN.example.com 的状态由之前的Ready 转化为Ready，SchedulingDisabled。

```shell
[root@master0 ~]# kubectl get node
NAME                  STATUS                     AGE
master0.example.com   Ready                      15d
nodea0.example.com    Ready                      15d
nodeb0.example.com    Ready,SchedulingDisabled   15d
```

之后再建立Pod，Kubernetes 集群将不再分配给Node 节点 nodebN.example.com 。

使用命令行直接操作也是可以的，命令为kubectl patch:

```shell
[root@master0 ~]# kubectl get node
NAME                  STATUS                     AGE
master0.example.com   Ready                      15d
nodea0.example.com    Ready                      15d
nodeb0.example.com    Ready,SchedulingDisabled   15d

[root@master0 ~]# kubectl patch node nodea0.example.com -p '{"spec": {"unschedulable": true}}'
"nodea0.example.com" patched

[root@master0 ~]# kubectl get node
NAME                  STATUS                     AGE
master0.example.com   Ready                      15d
nodea0.example.com    Ready,SchedulingDisabled   15d
nodeb0.example.com    Ready,SchedulingDisabled   15d

[root@master0 ~]# kubectl patch node nodea0.example.com -p '{"spec": {"unschedulable": false}}'
"nodea0.example.com" patched

[root@master0 ~]# kubectl patch node nodeb0.example.com -p '{"spec": {"unschedulable": false}}'
"nodeb0.example.com" patched

[root@master0 ~]# kubectl get node
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     15d
```

Kubernetes 1.4 中使用更加简洁的方式配置Node 节点的隔离和恢复，kubectl 新的子命令cordon和uncordon 同样可以完成隔离和恢复任务：

```shell
[root@master0 ~]# kubectl cordon nodeb0.example.com
node "nodeb0.example.com" cordoned
[root@master0 ~]# kubectl get node
NAME                  STATUS                     AGE
master0.example.com   Ready                      15d
nodea0.example.com    Ready                      15d
nodeb0.example.com    Ready,SchedulingDisabled   15d
[root@master0 ~]# kubectl uncordon nodeb0.example.com
node "nodeb0.example.com" uncordoned
[root@master0 ~]# kubectl get node
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     15d
```

#### Node节点的添加和剔除

首先，我们必须知道在Kubernetes 集群中，我们碰到最多且最棘手的问题是服务器容量不足，这时我们需要购买新的服务器，添加新的Node 节点，以达到系统水平扩展的目的。

在Kubernetes 1.4 集群中，添加新的节点非常简单，只需要在新Node 节点上安装 Kubernetes 1.4 软件，配置其基础运行环境后执行kubeadm jion 命令就可以将其添加到已经存在的Kubernetes 1.4 集群中。

接下来我们在现有的Kubernetes 1.4 集群中添加一台新的Node 节点 nodecN.example.com 。 在我们的实验环境中基础设备已经为大家预先配置。

启动nodec 虚拟机：

```shell
[kiosk@foundation0 Desktop]$ rht-vmctl reset nodec
Are you sure you want to reset nodec? (y/n) y
Powering off nodec.
Resetting nodec.
Creating virtual machine disk overlay for up500-nodec-vda.qcow2
Starting nodec.
```

连接nodec 虚拟机，请注意你的设备foundation号：

```shell
[kiosk@foundation0 Desktop]$ ssh root@nodec0
Last login: Sat Jun  4 14:39:46 2016 from 172.25.0.250
[root@nodec0 ~]#
```

按照基础课程中讲到的方法初始化Node 节点 nodecX.example.com 的环境

```shell
[root@nodec0 ~]# systemctl stop firewalld

[root@nodec0 ~]# systemctl  disable NetworkManager

[root@nodec0 ~]# systemctl  stop NetworkManager.service

[root@nodec0 ~]# setenforce 0

[root@nodec0 ~]# sed -i 's/SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config

[root@nodec0 ~]# yum install wget bzip2 net-tools -y

[root@nodec0 ~]# wget http://classroom.example.com/materials/kubernetes-1.4.repo -O /etc/yum.repos.d/k8s.rep

[root@nodec0 ~]# yum install docker-engine kubeadm kubectl kubelet kubernetes-cni -y

[root@nodec0 ~]# systemctl  enable docker  kubelet

[root@nodec0 ~]# systemctl  enable docker

[root@nodec0 ~]# wget http://classroom.example.com/materials/k8s-imgs/k8s-1.4-node-img.tbz

[root@nodec0 ~]# for i in ./k8s-1.4-node-img/*.img ; do docker load -i $i ; done

[root@nodec0 ~]# wget http://classroom.example.com/materials/k8s-imgs/heapster-img.tbz

[root@nodec0 ~]# tar -jxf heapster-img.tbz

[root@nodec0 ~]# for i in ./heapster/*.img ; do docker load -i $i ; done
```

将Node 节点 nodecX.example.com 添加入现有的Kubernetes 1.4 集群，请将token值替换为你自己的：

```shell
[root@nodec0 ~]# kubeadm join --token=0a349c.013fd0942f0c8498 172.25.0.10
<util/tokens> validating provided token
<node/discovery> created cluster info discovery client, requesting info from "http://172.25.0.10:9898/cluster-info/v1/?token-id=0a349c"
<node/discovery> cluster info object received, verifying signature using given token
<node/discovery> cluster info signature and contents are valid, will use API endpoints [https://172.25.0.10:443]
<node/csr> created API client to obtain unique certificate for this node, generating keys and certificate signing request
<node/csr> received signed certificate from the API server, generating kubelet configuration
<util/kubeconfig> created "/etc/kubernetes/kubelet.conf"

Node join complete:
* Certificate signing request sent to master and response
  received.
* Kubelet informed of new secure connection details.

Run 'kubectl get nodes' on the master to see this machine join.
```

在Master 节点上执行kubectl get node 可以看到新的节点已经加入：

```shell
[root@master0 ~]# kubectl get nodes
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     2h
nodec0.example.com    Ready     10s
```

需要注意的是token值，如果你忘记在之前基础课程创建Kubernetes 1.4 集群时备份token值了，也没有关系，在Master 节点执行如下指令，可以得到当前Kubernetes 1.4 集群的token值。

```shell
[root@master0 ~]# kubectl -n kube-system get secret clusterinfo -o yaml | grep token-map | awk '{print $2}' | base64 -d | sed "s|{||g;s|}||g;s|:|.|g;s/\"//g;" | xargs echo
0a349c.013fd0942f0c8498
```

剔除Node 节点的操作在生产环境中使用相对较少，我们采取的剔除步骤是，首先将其隔离，然后使用kubectl delete node 命令将其删除。但是要注意的是，我们这样做的目的是使其离线后修复其硬件故障，故障修复后重新启动这个Node 节点，节点会重新自动加回集群。

以下操作请注意将设备号设为你自己的设备号：

```shell
查看当前Node 节点状态
[root@master0 ~]# kubectl get node
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     20h
nodec0.example.com    Ready     18h

将Node 节点 nodec0.example.com 隔离
[root@master0 ~]# kubectl cordon nodec0.example.com
node "nodec0.example.com" cordoned

查看当前Node 节点状态
[root@master0 ~]# kubectl get node
NAME                  STATUS                     AGE
master0.example.com   Ready                      15d
nodea0.example.com    Ready                      15d
nodeb0.example.com    Ready                      20h
nodec0.example.com    Ready,SchedulingDisabled   18h

删除Node 节点 nodec0.example.com
[root@master0 ~]# kubectl delete node nodec0.example.com
node "nodec0.example.com" deleted
[root@master0 ~]# kubectl get node
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     20h

重启Node 节点 nodec0.example.com
[root@nodec0 ~]# reboot
Connection to nodec0 closed by remote host.
Connection to nodec0 closed.

Node 节点 nodec0.example.com 重启后会自动加回集群
[root@master0 ~]# kubectl get node
NAME                  STATUS    AGE
master0.example.com   Ready     15d
nodea0.example.com    Ready     15d
nodeb0.example.com    Ready     20h
nodec0.example.com    Ready     41s
```

#### Node 节点信息说明

我们有两种命令行获取Node 节点信息的方法： 

* kubectl get node Node_Name -o Output_Format
* kubectl describe node  Node_Name

首先看get 子命令方法：

```shell
YAML 格式输出
[root@master0 ~]# kubectl get node nodea0.example.com -o yaml
apiVersion: v1
kind: Node
metadata:
  annotations:
    volumes.kubernetes.io/controller-managed-attach-detach: "true"
  creationTimestamp: 2016-11-22T04:49:21Z
  labels:
    beta.kubernetes.io/arch: amd64
    beta.kubernetes.io/os: linux
    kubernetes.io/hostname: nodea0.example.com
  name: nodea0.example.com
  resourceVersion: "218660"
  selfLink: /api/v1/nodes/nodea0.example.com
  uid: 08a3d801-b06f-11e6-8ef8-52540000000a
spec:
  externalID: nodea0.example.com
status:
  addresses:
  - address: 172.25.0.11
    type: LegacyHostIP
  - address: 172.25.0.11
    type: InternalIP
  allocatable:
    alpha.kubernetes.io/nvidia-gpu: "0"
    cpu: "2"
    memory: 1016792Ki
    pods: "110"
  capacity:
    alpha.kubernetes.io/nvidia-gpu: "0"
    cpu: "2"
    memory: 1016792Ki
    pods: "110"
  conditions:
  - lastHeartbeatTime: 2016-12-08T03:28:08Z
    lastTransitionTime: 2016-11-25T06:59:35Z
    message: kubelet has sufficient disk space available
    reason: KubeletHasSufficientDisk
    status: "False"
    type: OutOfDisk
  - lastHeartbeatTime: 2016-12-08T03:28:08Z
    lastTransitionTime: 2016-11-22T04:49:21Z
    message: kubelet has sufficient memory available
    reason: KubeletHasSufficientMemory
    status: "False"
    type: MemoryPressure
  - lastHeartbeatTime: 2016-12-08T03:28:08Z
    lastTransitionTime: 2016-11-22T04:49:21Z
    message: kubelet has no disk pressure
    reason: KubeletHasNoDiskPressure
    status: "False"
    type: DiskPressure
  - lastHeartbeatTime: 2016-12-08T03:28:08Z
    lastTransitionTime: 2016-11-22T04:49:21Z
    message: kubelet is posting ready status
    reason: KubeletReady
    status: "True"
    type: Ready
  daemonEndpoints:
    kubeletEndpoint:
      Port: 10250
  images:
  - names:
    - kubernetes/heapster:canary
    sizeBytes: 1028534191
  - names:
    - kissingwolf/hpa-example:latest
    sizeBytes: 480748530
  - names:
    - kubernetes/heapster_influxdb:v0.6
    - kubernetes/heapster_influxdb@sha256:70b34b65def36fd0f54af570d5e72ac41c3c82e086dace9e6a977bab7750c147
    sizeBytes: 271083718
  - names:
    - gcr.io/google_containers/kube-proxy-amd64:v1.4.0
    sizeBytes: 202773136
  - names:
    - weaveworks/weave-kube:1.7.2
    sizeBytes: 196546754
  - names:
    - nginx:1.11.5
    - nginx:latest
    sizeBytes: 181440028
  - names:
    - nginx:1.10.2
    sizeBytes: 180664743
  - names:
    - gcr.io/google_containers/kubernetes-dashboard-amd64:v1.4.0
    sizeBytes: 86267953
  - names:
    - weaveworks/weave-npc:1.7.2
    sizeBytes: 76333700
  - names:
    - gcr.io/google_containers/pause-amd64:3.0
    sizeBytes: 746888
  nodeInfo:
    architecture: amd64
    bootID: 97cae03d-c66b-4719-b166-b2aad1ed02d5
    containerRuntimeVersion: docker://1.12.2
    kernelVersion: 3.10.0-327.el7.x86_64
    kubeProxyVersion: v1.4.0
    kubeletVersion: v1.4.0
    machineID: ecd4a28875734de9bf2cb5e40cbf88da
    operatingSystem: linux
    osImage: Red Hat Enterprise Linux Server 7.2 (Maipo)
    systemUUID: 9E450584-DC5A-4B7C-809E-25D67846B219

JSON 格式输出
[root@master0 ~]# kubectl get node nodea0.example.com -o json
{
    "kind": "Node",
    "apiVersion": "v1",
    "metadata": {
        "name": "nodea0.example.com",
        "selfLink": "/api/v1/nodes/nodea0.example.com",
        "uid": "08a3d801-b06f-11e6-8ef8-52540000000a",
        "resourceVersion": "218660",
        "creationTimestamp": "2016-11-22T04:49:21Z",
        "labels": {
            "beta.kubernetes.io/arch": "amd64",
            "beta.kubernetes.io/os": "linux",
            "kubernetes.io/hostname": "nodea0.example.com"
        },
        "annotations": {
            "volumes.kubernetes.io/controller-managed-attach-detach": "true"
        }
    },
    "spec": {
        "externalID": "nodea0.example.com"
    },
    "status": {
        "capacity": {
            "alpha.kubernetes.io/nvidia-gpu": "0",
            "cpu": "2",
            "memory": "1016792Ki",
            "pods": "110"
        },
        "allocatable": {
            "alpha.kubernetes.io/nvidia-gpu": "0",
            "cpu": "2",
            "memory": "1016792Ki",
            "pods": "110"
        },
        "conditions": [
            {
                "type": "OutOfDisk",
                "status": "False",
                "lastHeartbeatTime": "2016-12-08T03:28:08Z",
                "lastTransitionTime": "2016-11-25T06:59:35Z",
                "reason": "KubeletHasSufficientDisk",
                "message": "kubelet has sufficient disk space available"
            },
            {
                "type": "MemoryPressure",
                "status": "False",
                "lastHeartbeatTime": "2016-12-08T03:28:08Z",
                "lastTransitionTime": "2016-11-22T04:49:21Z",
                "reason": "KubeletHasSufficientMemory",
                "message": "kubelet has sufficient memory available"
            },
            {
                "type": "DiskPressure",
                "status": "False",
                "lastHeartbeatTime": "2016-12-08T03:28:08Z",
                "lastTransitionTime": "2016-11-22T04:49:21Z",
                "reason": "KubeletHasNoDiskPressure",
                "message": "kubelet has no disk pressure"
            },
            {
                "type": "Ready",
                "status": "True",
                "lastHeartbeatTime": "2016-12-08T03:28:08Z",
                "lastTransitionTime": "2016-11-22T04:49:21Z",
                "reason": "KubeletReady",
                "message": "kubelet is posting ready status"
            }
        ],
        "addresses": [
            {
                "type": "LegacyHostIP",
                "address": "172.25.0.11"
            },
            {
                "type": "InternalIP",
                "address": "172.25.0.11"
            }
        ],
        "daemonEndpoints": {
            "kubeletEndpoint": {
                "Port": 10250
            }
        },
        "nodeInfo": {
            "machineID": "ecd4a28875734de9bf2cb5e40cbf88da",
            "systemUUID": "9E450584-DC5A-4B7C-809E-25D67846B219",
            "bootID": "97cae03d-c66b-4719-b166-b2aad1ed02d5",
            "kernelVersion": "3.10.0-327.el7.x86_64",
            "osImage": "Red Hat Enterprise Linux Server 7.2 (Maipo)",
            "containerRuntimeVersion": "docker://1.12.2",
            "kubeletVersion": "v1.4.0",
            "kubeProxyVersion": "v1.4.0",
            "operatingSystem": "linux",
            "architecture": "amd64"
        },
        "images": [
            {
                "names": [
                    "kubernetes/heapster:canary"
                ],
                "sizeBytes": 1028534191
            },
            {
                "names": [
                    "kissingwolf/hpa-example:latest"
                ],
                "sizeBytes": 480748530
            },
            {
                "names": [
                    "kubernetes/heapster_influxdb:v0.6",
                    "kubernetes/heapster_influxdb@sha256:70b34b65def36fd0f54af570d5e72ac41c3c82e086dace9e6a977bab7750c147"
                ],
                "sizeBytes": 271083718
            },
            {
                "names": [
                    "gcr.io/google_containers/kube-proxy-amd64:v1.4.0"
                ],
                "sizeBytes": 202773136
            },
            {
                "names": [
                    "weaveworks/weave-kube:1.7.2"
                ],
                "sizeBytes": 196546754
            },
            {
                "names": [
                    "nginx:1.11.5",
                    "nginx:latest"
                ],
                "sizeBytes": 181440028
            },
            {
                "names": [
                    "nginx:1.10.2"
                ],
                "sizeBytes": 180664743
            },
            {
                "names": [
                    "gcr.io/google_containers/kubernetes-dashboard-amd64:v1.4.0"
                ],
                "sizeBytes": 86267953
            },
            {
                "names": [
                    "weaveworks/weave-npc:1.7.2"
                ],
                "sizeBytes": 76333700
            },
            {
                "names": [
                    "gcr.io/google_containers/pause-amd64:3.0"
                ],
                "sizeBytes": 746888
            }
        ]
    }
}
```

接下来看describe 子命令方法：

```shell
[root@master0 ~]# kubectl describe node nodea0.example.com
Name:			nodea0.example.com
Labels:			beta.kubernetes.io/arch=amd64
			beta.kubernetes.io/os=linux
			kubernetes.io/hostname=nodea0.example.com
Taints:			<none>
CreationTimestamp:	Tue, 22 Nov 2016 12:49:21 +0800
Phase:
Conditions:
  Type			Status	LastHeartbeatTime			LastTransitionTime			Reason				Message
  ----			------	-----------------			------------------			------				-------
  OutOfDisk 		False 	Thu, 08 Dec 2016 11:34:48 +0800 	Fri, 25 Nov 2016 14:59:35 +0800 	KubeletHasSufficientDisk 	kubelet has sufficient disk space available
  MemoryPressure 	False 	Thu, 08 Dec 2016 11:34:48 +0800 	Tue, 22 Nov 2016 12:49:21 +0800 	KubeletHasSufficientMemory 	kubelet has sufficient memory available
  DiskPressure 		False 	Thu, 08 Dec 2016 11:34:48 +0800 	Tue, 22 Nov 2016 12:49:21 +0800 	KubeletHasNoDiskPressure 	kubelet has no disk pressure
  Ready 		True 	Thu, 08 Dec 2016 11:34:48 +0800 	Tue, 22 Nov 2016 12:49:21 +0800 	KubeletReady 			kubelet is posting ready status
Addresses:		172.25.0.11,172.25.0.11
Capacity:
 alpha.kubernetes.io/nvidia-gpu:	0
 cpu:					2
 memory:				1016792Ki
 pods:					110
Allocatable:
 alpha.kubernetes.io/nvidia-gpu:	0
 cpu:					2
 memory:				1016792Ki
 pods:					110
System Info:
 Machine ID:			ecd4a28875734de9bf2cb5e40cbf88da
 System UUID:			9E450584-DC5A-4B7C-809E-25D67846B219
 Boot ID:			97cae03d-c66b-4719-b166-b2aad1ed02d5
 Kernel Version:		3.10.0-327.el7.x86_64
 OS Image:			Red Hat Enterprise Linux Server 7.2 (Maipo)
 Operating System:		linux
 Architecture:			amd64
 Container Runtime Version:	docker://1.12.2
 Kubelet Version:		v1.4.0
 Kube-Proxy Version:		v1.4.0
ExternalID:			nodea0.example.com
Non-terminated Pods:		(6 in total)
  Namespace			Name						CPU Requests	CPU Limits	Memory Requests	Memory Limits
  ---------			----						------------	----------	---------------	-------------
  kube-system			heapster-3901806196-8c2rj			0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			kube-proxy-amd64-ick75				0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			kubernetes-dashboard-1171352413-yuqpa		0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			monitoring-grafana-927606581-ruy2u		0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			monitoring-influxdb-3276295126-dmbtu		0 (0%)		0 (0%)		0 (0%)		0 (0%)
  kube-system			weave-net-fphro					20m (1%)	0 (0%)		0 (0%)		0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.
  CPU Requests	CPU Limits	Memory Requests	Memory Limits
  ------------	----------	---------------	-------------
  20m (1%)	0 (0%)		0 (0%)		0 (0%)
Events:
  FirstSeen	LastSeen	Count	From				SubobjectPath	Type		Reason			Message
  ---------	--------	-----	----				-------------	--------	------			-------
  49m		49m		1	{kubelet nodea0.example.com}		Normal		Starting		Starting kubelet.
  49m		48m		13	{kubelet nodea0.example.com}		Normal		NodeHasSufficientDisk	Node nodea0.example.com status is now: NodeHasSufficientDisk
  49m		48m		13	{kubelet nodea0.example.com}		Normal		NodeHasSufficientMemory	Node nodea0.example.com status is now: NodeHasSufficientMemory
  49m		48m		13	{kubelet nodea0.example.com}		Normal		NodeHasNoDiskPressure	Node nodea0.example.com status is now: NodeHasNoDiskPressure
  48m		48m		1	{kubelet nodea0.example.com}		Warning		Rebooted		Node nodea0.example.com has been rebooted, boot id: 97cae03d-c66b-4719-b166-b2aad1ed02d5
  47m		47m		1	{kube-proxy nodea0.example.com}		Normal		Starting		Starting kube-proxy.
```

Node 节点信息输出很清晰，很容易理解其意义。上课时我们会逐步介绍每部分的含义和作用。

### Namespace 名字空间管理

在Kubernetes 集群中，环境共享和隔离是通过Linux Namespace 名字空间管理来完成的。

生产环境下，公司会有很多需要线上运行资源的部门，大公司会有电商事业部、网游事业部、手游事业部和门户事业部等等，小公司会有开发部、测试部和运维部等等，创业公司也会有开发测试组和安全运维支持组等等。不同的部门和工作组使用同一套Kubernetes 集群环境是最经济和环保的选择，但是如何做到不同部门和工作组之间互不干扰和睦相处呢？这时就需要在Kubernetes 集群中划分不同的运行隔离环境，将不同的部门和工作组限制在不同的隔离环境中，使其创建、修改和删除资源对象的操作相对独立，这样也有利于公司内部实现敏捷开发和敏捷部署，并减少由于部门间的误操作导致故障。

#### 默认Namespace

Kubernetes 1.4 中默认已经创建了两个Namespace：

* default
* kube-system

我们可以通过kubectl get namespace 命令查看到：

```shell
[root@master0 ~]# kubectl get namespace
NAME          STATUS    AGE
default       Active    16d
kube-system   Active    16d

[root@master0 ~]# kubectl get namespace -o yaml
apiVersion: v1
items:
- apiVersion: v1
  kind: Namespace
  metadata:
    creationTimestamp: 2016-11-22T04:48:50Z
    name: default
    resourceVersion: "6"
    selfLink: /api/v1/namespaces/default
    uid: f6501df8-b06e-11e6-8ef8-52540000000a
  spec:
    finalizers:
    - kubernetes
  status:
    phase: Active
- apiVersion: v1
  kind: Namespace
  metadata:
    creationTimestamp: 2016-11-22T04:48:50Z
    name: kube-system
    resourceVersion: "9"
    selfLink: /api/v1/namespaces/kube-system
    uid: f66c62f8-b06e-11e6-8ef8-52540000000a
  spec:
    finalizers:
    - kubernetes
  status:
    phase: Active
kind: List
metadata: {}
```

Namespace 名字空间的具体信息可以使用kubectl describe namespace 命令查看：

```shell
[root@master0 ~]#  kubectl describe namespace
Name:	default
Labels:	<none>
Status:	Active

No resource quota.

No resource limits.


Name:	kube-system
Labels:	<none>
Status:	Active

No resource quota.

No resource limits.
```

当前系统没有预设置的情况下，我们都是工作在default 名字空间的，在早期的Kubernetes 集群中如果设置过其他名字空间并设置其为默认名字空间，可以使用kubectl namespace 子命令查看：

```shell
[root@master0 ~]# kubectl namespace
error: namespace has been superseded by the context.namespace field of .kubeconfig files.  See 'kubectl config set-context --help' for more details.
```

以上回显就是没有设置默认名字空间，同时也提示了设置用户默认名字空间的方法。

到Kubernetes 1.4的时候，kubectl namespace 命令已经被弃用了，你可以看到如下信息：

```shell
[root@master0 ~]# kubectl namespace -h
Deprecated: This command is deprecated, all its functionalities are covered by "kubectl config set-context"
Usage:
  kubectl namespace [namespace] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

Kubernetes 1.4 以后，建议使用kubectl config set-context 来完成名字空间的所有操作。

kube-system 名字空间是Kubernetes 1.4 集群系统环境的名字空间，尽量不要去自定义配置这个名字空间内的运行环境和资源。

```shell
[root@master0 ~]# kubectl get pod --namespace=kube-system
NAME                                          READY     STATUS    RESTARTS   AGE
etcd-master0.example.com                      1/1       Running   8          16d
heapster-3901806196-8c2rj                     1/1       Running   6          14d
kube-apiserver-master0.example.com            1/1       Running   14         16d
kube-controller-manager-master0.example.com   1/1       Running   8          16d
kube-discovery-982812725-nghjq                1/1       Running   8          16d
kube-dns-2247936740-f32d9                     3/3       Running   24         16d
kube-proxy-amd64-ick75                        1/1       Running   3          12d
kube-proxy-amd64-px2ms                        1/1       Running   6          14d
kube-proxy-amd64-t93vd                        1/1       Running   1          23h
kube-proxy-amd64-xg7bg                        1/1       Running   0          2h
kube-scheduler-master0.example.com            1/1       Running   10         16d
kubernetes-dashboard-1171352413-yuqpa         1/1       Running   6          14d
monitoring-grafana-927606581-ruy2u            1/1       Running   0          23h
monitoring-influxdb-3276295126-dmbtu          1/1       Running   1          23h
weave-net-b1z6l                               2/2       Running   1          2h
weave-net-fphro                               2/2       Running   6          12d
weave-net-fqman                               2/2       Running   4          23h
weave-net-kpvob                               2/2       Running   12         14d

[root@master0 ~]# kubectl get deployment --namespace=kube-system
NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
heapster               1         1         1            1           14d
kube-discovery         1         1         1            1           16d
kube-dns               1         1         1            1           16d
kubernetes-dashboard   1         1         1            1           16d
monitoring-grafana     1         1         1            1           14d
monitoring-influxdb    1         1         1            1           14d

[root@master0 ~]# kubectl get service --namespace=kube-system
NAME                   CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
heapster               100.77.26.79     <none>        80/TCP          14d
kube-dns               100.64.0.10      <none>        53/UDP,53/TCP   16d
kubernetes-dashboard   100.77.127.110   <nodes>       80/TCP          16d
monitoring-grafana     100.70.101.80    <nodes>       80/TCP          14d
monitoring-influxdb    100.64.163.255   <none>        8086/TCP        14d
```

我们通过--namespace 参数可以在kubectl 命令中指定操作的名字空间，默认不指定的时候是default名字空间。

#### 创建自定义Namespace

创建自定义的名字空间有两种命令行方法：

* 通过配置文件创建
* 命令行直接创建

我们在接下来的实验中将建立两个新的名字空间，分别分配给开发组development和运维组systemadm。

##### 配置文件创建Namespace

我们使用配置文件创建开发组development的名字空间development ，配置文件名为namespace-dev.yaml，内容如下：

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: development
```

然后使用 kubectl create 命令创建名字空间：

```shell
[root@master0 ~]# kubectl create -f namespace-dev.yaml
namespace "development" created

[root@master0 ~]# kubectl get namespace
NAME          STATUS    AGE
default       Active    16d
development   Active    5s
kube-system   Active    16d
```

可以看到新的名字空间development 已经创建。

##### 命令行创建Namespace

命令行创建使用命令kubectl create namespace Namespace_name 就可以了，接下来我们创建名字空间systemadm :

```shell
[root@master0 ~]# kubectl  create namespace systemadm
namespace "systemadm" created

[root@master0 ~]# kubectl get namespace
NAME          STATUS    AGE
default       Active    16d
development   Active    2m
kube-system   Active    16d
systemadm     Active    56s
```

可以看到很方便的就创建好了。

#### 创建Context运行环境

仅仅有名字空间并不能方便我们对用户和组的管理，我们还需要设置Context 运行环境，并且设置Context和名字空间的关联，这样组织结构就明细了。

我们创建两个运行环境：uplooking-dev和uplooking-sysadm，并将其关联到之前创建的两个名字空间，使用的命令是kubctl config set-context :

```shell
[root@master0 ~]# kubectl config  set-context uplooking-dev --namespace=development
context "uplooking-dev" set.

[root@master0 ~]# kubectl config view
apiVersion: v1
clusters: []
contexts:
- context:
    cluster: ""
    namespace: development
    user: ""
  name: uplooking-dev
current-context: ""
kind: Config
preferences: {}
users: []

[root@master0 ~]# kubectl config  set-context uplooking-sysadm --namespace=systemadm
context "uplooking-sysadm" set.

[root@master0 ~]# kubectl config view
apiVersion: v1
clusters: []
contexts:
- context:
    cluster: ""
    namespace: development
    user: ""
  name: uplooking-dev
- context:
    cluster: ""
    namespace: systemadm
    user: ""
  name: uplooking-sysadm
current-context: ""
kind: Config
preferences: {}
users: []
```

kubectl config 命令其实就是创建和配置 ~/.kube/config 文件，kubectl config view 显示的也是此文件。因此，你也可以手工修改此文件。

#### 切换Context 运行环境

我们可以通过kubectl config get-contexts 命令查看当前用户所处的运行环境，可以通过kubectl config use-context 命令切换用户所处的运行环境。每个运行环境的名字空间各不相同，各自之间不像话影响。

```shell
[root@master0 ~]# kubectl config use-context uplooking-dev
switched to context "uplooking-dev".

[root@master0 ~]# kubectl config get-contexts
CURRENT   NAME               CLUSTER   AUTHINFO   NAMESPACE
          uplooking-sysadm                        systemadm
*         uplooking-dev                           development

[root@master0 ~]# kubectl config use-context uplooking-sysadm
switched to context "uplooking-sysadm".

[root@master0 ~]# kubectl config get-contexts
CURRENT   NAME               CLUSTER   AUTHINFO   NAMESPACE
          uplooking-dev                           development
*         uplooking-sysadm                        systemadm
```

由于kubectl config 是通过操作配置文件 ~/.kube/config 来切换环境的，所以你重新连接或重新启动都不会影响你上次设置的运行环境。

#### 独立Context运行环境测试

首先查看当前Context 运行环境：

```shell
[root@master0 ~]# kubectl config get-contexts
CURRENT   NAME               CLUSTER   AUTHINFO   NAMESPACE
*         uplooking-sysadm                        systemadm
          uplooking-dev                           development
```

我们当前使用的uplooking-sysadm 运行环境，在systemadm 名字空间中。

此时创建的Kubernetes 集群资源都工作在systemadm名字空间中，我们创建一个服务测试一下，服务的配置文件名为my-nginx.yaml ，其内容如下：

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  ports:
  - port: 8000
    targetPort: 80
    protocol: TCP
  selector:
    app: nginx
  type: LoadBalancer
```

通过kubectl create 命令创建：

```shell
[root@master0 ~]# kubectl create -f  my-nginx.yaml
deployment "nginx-deployment" created
service "nginx-service" created
```

通过 kubectl get 命令查看信息:

```shell
[root@master0 ~]# kubectl get pod
NAME                                READY     STATUS              RESTARTS   AGE
nginx-deployment-2273492681-5nykp   1/1       Running             0          12s
nginx-deployment-2273492681-6lv22   0/1       ContainerCreating   0          12s
[root@master0 ~]# kubectl get service
NAME            CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
nginx-service   100.66.253.187   <pending>     8000/TCP   21s
[root@master0 ~]# kubectl get deployment
NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2         2         2            1           29s
```

这时，如果我们切换Context 运行环境到uplooking-dev ，我们就会发现默认环境下没有任何Pod 、Service和Deployment:

```shell
[root@master0 ~]# kubectl config use-context uplooking-dev
switched to context "uplooking-dev".
[root@master0 ~]# kubectl get pod
[root@master0 ~]# kubectl get service
[root@master0 ~]# kubectl get deployment
```

这样开发组和运维组就相对独立的配置和管理自己的资源环境了。

当然，作为管理人员，我们还是可以看到所有名字空间下运行的资源的。你需要在命令后加上--all-namespaces， 就像这样：

```shell
[root@master0 ~]# kubectl get pod --all-namespaces
NAMESPACE     NAME                                          READY     STATUS             RESTARTS   AGE
kube-system   etcd-master0.example.com                      1/1       Running            8          16d
kube-system   heapster-3901806196-8c2rj                     1/1       Running            6          14d
kube-system   kube-apiserver-master0.example.com            1/1       Running            14         16d
kube-system   kube-controller-manager-master0.example.com   1/1       Running            8          16d
kube-system   kube-discovery-982812725-nghjq                1/1       Running            8          16d
kube-system   kube-dns-2247936740-f32d9                     3/3       Running            24         16d
kube-system   kube-proxy-amd64-ick75                        1/1       Running            3          12d
kube-system   kube-proxy-amd64-px2ms                        1/1       Running            6          14d
kube-system   kube-proxy-amd64-t93vd                        1/1       Running            1          1d
kube-system   kube-proxy-amd64-xg7bg                        1/1       Running            0          3h
kube-system   kube-scheduler-master0.example.com            1/1       Running            10         16d
kube-system   kubernetes-dashboard-1171352413-yuqpa         1/1       Running            6          15d
kube-system   monitoring-grafana-927606581-ruy2u            1/1       Running            0          1d
kube-system   monitoring-influxdb-3276295126-dmbtu          1/1       Running            1          1d
kube-system   weave-net-b1z6l                               2/2       Running            1          3h
kube-system   weave-net-fphro                               2/2       Running            6          12d
kube-system   weave-net-fqman                               2/2       Running            4          1d
kube-system   weave-net-kpvob                               2/2       Running            12         14d
systemadm     nginx-deployment-2273492681-5nykp             1/1       Running            0          6m
systemadm     nginx-deployment-2273492681-6lv22             0/1       ImagePullBackOff   0          6m
```

回显部分就会标注是哪个Namespace 名字空间里运行的Kubernetes 集群资源。





