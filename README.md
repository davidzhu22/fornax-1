# Fornax Bash Scripts for Deployment

# * Clone the repository into Node-A 
git clone https://github.com/click2cloud-prajwal/Scripts.git

# * Edit the IP's in the script
export a= <ip address of node-a>
export b= <ip address of node-b>
export c= <ip address of node-c>

# * Run the Script
sudo bash Scripts/fornax1.sh (for node-a run the 'fornax1.sh'. Simultaneously run the 'fornax2.sh' for node-b and 'fornax3.sh' for node-c.
  
# * Verify the Edgecluster in Node-A
kubectl get edgecluster

