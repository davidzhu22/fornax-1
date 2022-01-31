# Cloud Intel Deployment On Single VM


### Abstract


The purpose of this document is to setup and configure the **Cloud Intel** on single virtual machine. Cloud Intel is an assessment tool that identifies your IT inventory and evaluates it to make you decide the movement of your workloads to Cloud. 

## 1.1. Virtual Machine Configuration


•      ` Centos 7 `                                                                                                                              
•      ` 8 vCPUs, 16 GB RAM and 200 GB Storage `                                                                

## 1.2. Install Kubernetes Tools 


•      Disable SELinux and Disable swap                                                                                          
•      Letting iptables see bridged traffic                                                                                       
•      Install docker runtime                                                                                                        
•      Installing kubeadm, kubelet and kubectl                                                                                   


### 1.2.1 Installing Docker 

Link of commands to [Install Docker](https://phoenixnap.com/kb/how-to-install-docker-centos-7)

### 1.2.2. Installing kubeadm, kubelet and kubectl


You will install these packages on your machine:

• **kubeadm:** the command to bootstrap the cluster.

• **kubelet:** the component that runs on all of the machines in your cluster and does things like starting pods and containers.

• **kubectl:** the command line util to talk to your cluster.

Link of commands to [Install Kubernetes on CentOS 7](https://phoenixnap.com/kb/how-to-install-kubernetes-on-centos#:~:text=1%20Step%201%3A%20Create%20Cluster%20with%20kubeadm.%20The,options.%204%20Step%204%3A%20Check%20Status%20of%20Cluster) 

• **Make a host entry or DNS record to resolve the hostname for all node:

     vi /etc/hosts
     

### 1.2.3. Start a cluster using kubeadm


• Run command (it might cost a few minutes)

      sudo kubeadm init --pod-network-cidr=10.244.0.0/16
      
• At the end of the screen output, you will see info about setting the kubeconfig. Do the following if you are the root user:

      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config

      export KUBECONFIG=/etc/kubernetes/admin.conf
      
• Set Up Pod Network
   
      sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

• Check the cluster is up by running some commands, like

      kubectl get nodes
      

 
 

