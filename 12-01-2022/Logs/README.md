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


### 1.2.1. Letting iptables see bridged traffic

•  Make sure that the br_netfilter module is loaded. This can be done by running lsmod | grep br_netfilter. To load it explicitly call sudo modprobe br_netfilter.

        sudo modprobe br_netfilter
        lsmod | grep br_netfilter

        cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
        br_netfilter
        EOF

        cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
        net.bridge.bridge-nf-call-ip6tables = 1
        net.bridge.bridge-nf-call-iptables = 1
        EOF
        sudo sysctl --system
        
        
•  Verify the bridged

       lsmod | grep br_netfilter
       
### 1.2.2. Install Docker Runtime


•  Install Docker runtime 
      
      sudo apt-get update
      sudo apt-get install docker.io -y
      
### 1.2.3. Installing kubeadm, kubelet and kubectl


You will install these packages on your machine:

• **kubeadm:** the command to bootstrap the cluster.

• **kubelet:** the component that runs on all of the machines in your cluster and does things like starting pods and containers.

• **kubectl:** the command line util to talk to your cluster.


i. Update the apt package index and install packages needed to use the Kubernetes apt repository:
    
      sudo apt-get update
      sudo apt-get install -y apt-transport-https ca-certificates curl
      
ii. Download the Google Cloud public signing key:

      sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
      
iii. Add the Kubernetes apt repository:

      echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
      
iv. Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:
   
     sudo apt-get update
     apt-get install -qy kubelet=1.21.1-00 kubectl=1.21.1-00 kubeadm=1.21.1-00
     sudo apt-mark hold kubelet kubeadm kubectl
     
• Next, run the command to enable docker service **systemctl enable docker.service**

### 1.2.4. Start a cluster using kubeadm


• Run command (it might cost a few minutes)

      kubeadm init
      
• At the end of the screen output, you will see info about setting the kubeconfig. Do the following if you are the root user:

      export KUBECONFIG=/etc/kubernetes/admin.conf

• Check the cluster is up by running some commands, like

      kubectl get nodes
      

 
 

