# Cloud Intel Deployment On Single VM


### Abstract


The purpose of this document is to setup and configure the **Cloud Intel** on single virtual machine. Cloud Intel is an assessment tool that identifies your IT inventory and evaluates it to make you decide the movement of your workloads to Cloud. 

##  Virtual Machine Configuration


•      ` Centos 7 `                                                                                                                              
•      ` 8 vCPUs, 16 GB RAM and 200 GB Storage `                                                                

### 1. Installing Kubernetes using Kubespray: 
                                                                                   
       yum upgrade -y
       git clone https://github.com/Click2Cloud/cb-deployment-assets.git
       yum install epel-release -y
       sudo yum install ansible
       
### 2. Generate ssh-key:

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

       
### 3. Configure Kubespray

       cd cb-deployment-assets/cloudintel_single_vm_deployment/kubespray
       sudo pip3 install -r requirements.txt
       cp -rfp inventory/sample inventory/mycluster

**Update your inventory/mycluster/inventry.ini with VM IP Address file**

       vi inventory/mycluster/inventory.ini
       
   ![image](https://user-images.githubusercontent.com/95343388/151781654-6be45601-e6f2-4b82-b968-81e3c5fb880e.png)
   
   
**Declare the IP Address:**

       declare -a IPS=(**IP ADDRESS**)
       CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
       
      
### 4. Deploy Kubespray with Ansible Playbook - run the playbook as root:
  
      ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml


• Check the cluster is up by running some commands, like

      kubectl get nodes

       
### 5. Apply the YAML's for deployment:

      cd /root/cb-deployment-assets/cloudintel_single_vm_deployment/client_Deployment/cloudintel_prod_client
      kubectl apply -f 00-namespace.yml
      kubectl apply -f 02-configmaps.yml
      kubectl apply -f 02-secrets.yml
      kubectl apply -f 04-ci-app.yml
      kubectl apply -f 04-ci-node.yml
      kubectl apply -f 04-ci-python-api.yml
      
**Similarly apply**
   
      cd /root/cb-deployment-assets/cloudintel_single_vm_deployment/client_Deployment/wiui_client/prod
      kubectl apply -f 01-namespace.yml
      kubectl apply -Rf configmap-secrets/*
      kubectl apply -Rf pv-pvc/*
      kubectl apply -Rf deployments-services/*
 

