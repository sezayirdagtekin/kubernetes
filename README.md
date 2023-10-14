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

## Enable addons
With microk8s status you can see the list of available addons and the ones currently enabled.

```
sudo microk8s status
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/88c5a404-bbc8-47aa-8a92-04fae1e86831)

By default we get a barebones upstream Kubernetes. Additional services, such as
dashboard  can be enabled by running the command

```
sudo microk8s enable dashboard
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/bfd0b90c-ff17-43c7-90d1-70938d0afaeb)


dashboard  can be disable by running the command

```
 sudo microk8s disable dashboard
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/1c1fd9fb-bc01-424c-9caf-38af5f1318fc)

### Install kubectl

To install kubectl, simply use the following command:
```
 sudo snap install kubectl --classic
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/3d18efb9-9882-4660-bfd8-45d7cb3925d1)


### Permissions
To avoid the need to use sudo when running microk8s commands, add the current user to the microk8s group and ensure that the user has access to files in the ~/.kube folder.

```
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
```


### IGenerate Kube config
Backup any existing Kube configuration file and then generate a new kube config.

```
mkdir -p ~/.kube
microk8s config > ~/.kube/config
```

![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/7b02003d-06e2-4479-b359-69ff87ceaad9)

After reboot

![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/928ae9c0-3ab2-4719-8fa6-7c416179254a)




