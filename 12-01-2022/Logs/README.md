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


### 1.2.1. Disable SELinux and swap

• Disable SELinux
       setenforce 0
       sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
       
• Disable swap
       swapoff -a
      
      
• For our next trick, we'll be enabling the br_netfilter kernel module
        
        modprobe br_netfilter
        echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
       

### 1.2.2. Install Docker Runtime

        yum install -y yum-utils device-mapper-persistent-data lvm2
        yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
        yum install -y docker-ce
      
### 1.2.3. Installing kubeadm, kubelet and kubectl


You will install these packages on your machine:

• **kubeadm:** the command to bootstrap the cluster.

• **kubelet:** the component that runs on all of the machines in your cluster and does things like starting pods and containers.

• **kubectl:** the command line util to talk to your cluster.

i. First we need to create a repository entry for yum. To do this, issue the command `vi /etc/yum.repos.d/kubernetes.repo` and then add the following contents:
       
       `[kubernetes]
       name=Kubernetes
       baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
       enabled=1
       gpgcheck=1
       repo_gpgcheck=1
       gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
               https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg`
               
               
ii. Save and close that file. Install Kubernetes with the command:

     yum install -y kubelet kubeadm kubectl
      
iii. **Once the installation completes, reboot all three machines. As soon as each machine has rebooted, log back in and su to the root user.**
     
• Next, run the command to enable docker service **systemctl enable docker.service**

### 1.2.4. Start a cluster using kubeadm


• Run command (it might cost a few minutes)

      sudo kubeadm init --pod-network-cidr=10.244.0.0/16
      
• At the end of the screen output, you will see info about setting the kubeconfig. Do the following if you are the root user:

      export KUBECONFIG=/etc/kubernetes/admin.conf
      
• Set Up Pod Network
   
      sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

• Check the cluster is up by running some commands, like

      kubectl get nodes
      

 
 

