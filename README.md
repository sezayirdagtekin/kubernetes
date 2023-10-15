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


### IGenerate Kube config
Backup any existing Kube configuration file and then generate a new kube config.


###  Accessing the Kubernetes dashboard

you can install the dashboard
```
microk8s enable dashboard
```
create token

```
microk8s kubectl create token default
```

get token 
```
microk8s kubectl -n kube-system describe secret $token
```

you can also reach the dashboard by forwarding its port to a free one on your host with:
```
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 9444:443
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/fde68ef9-6594-49ac-910d-76b44f0d3bf5)

you can then access the Dashboard at https://127.0.0.1:10444.
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/5b764f4a-0e00-472f-88e8-d3f07da0b936)

 ### Deploy NGINX web server Application with MicroK8s
Log in to the MicroK8s controller. Lets deploy an NGINX web server application. We’ll name this deployment nginx-webserver and use the official NGINX container image for the deployment. The command for this is:

```
 microk8s kubectl create deployment nginx-webserver --image=nginx
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/8595940a-3464-4ce7-8431-c020cefcddca)


Verify the deployment was successful with the command:

```
microk8s kubectl get pods
```

![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/0e6aebd1-254c-463a-87db-bc047505a5c8)

Pod has been deployed to the cluster. What is a pod? A Kubernetes pod is a collection of one or more containers and is the smallest unit of an application. Pods are IIIn In our instance above, we deployed a pod with a single container (NGINX).

At this point, the NGINX application is running but isn’t accessible. In order to make it accessible, we  have to deploy a service. What we’ll do here is expose our nginx-webserver deployment, using the type “NodePort” on port 80. NodePort is an open port on every node connected to your cluster. Kubernetes routes incoming traffic on the NodePort to your deployed service or application.

To deploy the service, the command will look like this

```
microk8s kubectl expose deployment nginx-webserver --type="NodePort" --port 80
```
![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/a2a2e589-d619-4a96-9e9f-9df7c3746350)


``` microk8s kubectl get service
```

As you can see, Kubernetes has mapped internal port 80 (the one being used by NGINX) to external port 32729 (the one being used by the Kubernetes cluster). So, if you point your web browser to localhost:32729, you should see the NGINX welcome screen in your browser.

![image](https://github.com/sezayirdagtekin/microk8s/assets/6317282/1e3753cd-9745-47f6-9064-de764b59bcb6)



