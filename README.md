# Is it cat ? Azure version.

This repo sets up your exisitng Ansible Tower install to deploy the "Is It Cat?" application in Azure.

"Is It Cat" is a simple image claissification Python app that comes with 2 (pre-trained) model versions: one that only recognizes cars (and thinks that some cats are cars), and one that recognizes cars and cats. 

What you need   
---

A Tower setup with an isolated node in Azure. This node needs git to be installed and the jmespath pip module in the default venv.

Go to the  `lab/` directory and complete up the `lab-credentials.yml.txt` file.  

Rename it to `lab-credentials.secret.yml`
<BR>Encrypt it with ansible-vault if you feel like it.  

Run: `ansible-playbook --vault-password-file=vault-password.secret.txt tower_setup.yml`

This will create an organization called "meteor", a project, an inventory, credential types, credentials and the template needed for the demo.

Run the demo
---

Just run the tower job template named: `ML Cat - Azure (dev) - Provision`
 
This will create the needed Azure components: ressource groups, virtnet, storage account and blob storage, loadbalancer, nics, and (by default, 2 VMs). You can use extra_vars to override the default values found in the default of the Azure role that is used (see: `requirements.yml`). 
 
It will then download and install the specified version of the "Is it cat" application and its appropriate TensorFlow model on both VMs. 
 
The extra_vars configured in the template are:
<BR>`app`: the url for the "Is it cat" app
<BR>`version`: the git tag of the app that will be deployed
<BR>`az_count`: the number of instances you want (scale up an existing deployement)
<BR>`az_vm_size`: the azure size of the VMs
<BR>`object_store`: tells the app which storage backend to use, for azure it's `blob` 
<BR>`scope`: the prefix that will get use in azure for all the VMs


