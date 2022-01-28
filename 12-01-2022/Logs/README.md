# Cloud Intel Deployment On Single VM


### Abstract


The purpose of this document is to setup and configure the **Cloud Intel** on single virtual machine. Cloud Intel is an assessment tool that identifies your IT inventory and evaluates it to make you decide the movement of your workloads to Cloud. 

## 1.1. Virtual Machine Configuration


•      ` Centos 7 `                                                                                                                              
•      ` 8 vCPUs, 16 GB RAM and 200 GB Storage `                                                                

## 1.2. Install Kubernetes Tools 


•      Install kubernetes tools to virtual machine.                                                                                          
•      Letting iptables see bridged traffic                                                                                       
•      Install docker runtime                                                                                                        
•      Installing kubeadm, kubelet and kubectl                                                                                   


### 1.2.1. Configure Kubernetes Repository and Letting iptables see bridged traffic


• Configure Kubernetes Repository
     
 Create `/etc/yum.repos.d/kubernetes.repo` and paste the below commands:
 
       [kubernetes]
       name=Kubernetes
       baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
       enabled=1
       gpgcheck=1
       repo_gpgcheck=1
       gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
      
      
•  Make sure that the br_netfilter module is loaded. This can be done by running lsmod | grep br_netfilter. To load it explicitly call sudo modprobe br_netfilter.

       cat <<EOF > /etc/sysctl.d/k8s.conf
       net.bridge.bridge-nf-call-ip6tables = 1
       net.bridge.bridge-nf-call-iptables = 1
       EOF
       sysctl --system
        
       
### 1.2.2. Install Docker Runtime

  
• Disable SELinux  
  
      sudo setenforce 0
      sudo sed -i ‘s/^SELINUX=enforcing$/SELINUX=permissive/’ /etc/selinux/config
      
• Disable swap    
    
      sudo sed -i '/swap/d' /etc/fstab
      sudo swapoff -a
      
• Configure Firewall

      sudo firewall-cmd --permanent --add-port=6443/tcp
      sudo firewall-cmd --permanent --add-port=2379-2380/tcp
      sudo firewall-cmd --permanent --add-port=10250/tcp
      sudo firewall-cmd --permanent --add-port=10251/tcp
      sudo firewall-cmd --permanent --add-port=10252/tcp
      sudo firewall-cmd --permanent --add-port=10255/tcp
      sudo firewall-cmd --reload
      
•  Install Docker runtime 
      
      sudo yum update -y
      sudo yum install docker.io -y
      
### 1.2.3. Installing kubeadm, kubelet and kubectl


You will install these packages on your machine:

• **kubeadm:** the command to bootstrap the cluster.

• **kubelet:** the component that runs on all of the machines in your cluster and does things like starting pods and containers.

• **kubectl:** the command line util to talk to your cluster.


i. Update the apt package index and install packages needed to use the Kubernetes apt repository:
    
      sudo yum update
      sudo yum install -y apt-transport-https ca-certificates curl
      
ii. Download the Google Cloud public signing key:

      sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
      
iii. Add the Kubernetes apt repository:

      echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
      
iv. Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:
   
     sudo yum update
     yum install -qy kubelet=1.21.1-00 kubectl=1.21.1-00 kubeadm=1.21.1-00
     systemctl enable kubelet
     systemctl start kubelet
     
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
      

 
 

