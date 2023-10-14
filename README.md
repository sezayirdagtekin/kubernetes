MicroK8s is a CNCF certified upstream Kubernetes deployment that runs entirely on your workstation or edge device. Being a snap it runs all Kubernetes services natively (i.e. no virtual machines) while packing the entire set of libraries and binaries needed. Installation is limited by how fast you can download a couple of hundred megabytes and the removal of MicroK8s leaves nothing behind.

## Install a local Kubernetes with MicroK8s

```
sudo snap install microk8s --classic
```

![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/b83ee7f2-f04e-400d-adc9-c5ded0d2cc14)

MicroK8s is a snap and as such it is frequently updated to each release of Kubernetes. To follow a specific upstream release series it’s possible to select a channel during installation. For example, to follow the v1.2 series:


```
sudo snap install microk8s --classic --channel=1.26/stable
```

Channels are made up of a track and an expected level of MicroK8s’ stability. Try snap info microk8s to see what versions are currently published. At the time of this writing

```
 snap info microk8s
```

![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/8ba37add-5d0f-4433-b8ef-a67967252e81)


![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/63cf27c5-936b-4c88-9fd4-d375362478bb)

You may need to configure your firewall to allow pod-to-pod and pod-to-internet communication:

```
sudo ufw allow in on cni0 && sudo ufw allow out on cni0
sudo ufw default allow routed
```



