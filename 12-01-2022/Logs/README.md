# Cloud Intel Deployment On Single VM


### Abstract


The purpose of this document is to setup and configure the **Cloud Intel** on single virtual machine. Cloud Intel is an assessment tool that identifies your IT inventory and evaluates it to make you decide the movement of your workloads to Cloud. 

## 1.1. Virtual Machine Configuration


•      ` Centos 7 `                                                                                                                              
•      ` 8 vCPUs, 16 GB RAM and 200 GB Storage `                                                                

### 1. Installing Kubernetes using Kubespray: 
                                                                                   
       yum upgrade -y
       git clone 
       yum install epel-release -y
       sudo yum install ansible
       
### 1.1 Generate ssh-key:

**Edit the ssh file and uncomments the below parameters**

       vi /etc/ssh/sshd_config
       
`Uncomment the #PermitRootLogin yes`
`Uncomment the  #PasswordAuthentication yes`

       systemctl reload sshd

**Now generate the key**

       ssh-keygen
       ssh-copy-id `Enter IP address of Machine`
  
   ![image](https://user-images.githubusercontent.com/95343388/151780812-01a8cd61-8151-46aa-b42c-c4da611b272e.png)

       
**Install the prerequisite packages:**

       yum install wget openssh* curl wget vim git python-pip -y

       
### 1.2. Configure Kubespray

       cd kubespray
       sudo pip3 install -r requirements.txt
       cp -rfp inventory/sample inventory/mycluster

**Update your inventory/mycluster/inventry.ini with VM IP Address file**

       vi inventory/mycluster/inventory.ini
       
   ![image](https://user-images.githubusercontent.com/95343388/151781654-6be45601-e6f2-4b82-b968-81e3c5fb880e.png)
   
   

###  Deploy Kubespray with Ansible Playbook - run the playbook as root:
  
      ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml


• Check the cluster is up by running some commands, like

      kubectl get nodes
      
### Clone the Deployment YAML's

       
### Apply the YAML's for deployment:

      
 
 

