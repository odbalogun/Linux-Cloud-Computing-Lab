1. Supported SSH Key formats:
    Azure supports SSH protocol 2 (SSH-2) RSA public-private key pairs with a minimum length of 2048 bits.

2. Create an SSH key pair:

        ssh-keygen -m PEM -t rsa -b 4096

3. Create and attach SSH key when creating VM:

        az vm create --resource-group genericRG --name firstVM --image UbuntuLTS --admin-username sysuser2 --size Standard_B1ls --generate-ssh-keys

4. Provide an SSH key when deploying a VM:

        az vm create --resource-group genericRG --name secondVM --image UbuntuLTS --admin-username sysuser2 --ssh-key-values ~/.ssh/id_rsa.pub

5. SSH into VM:

        ssh sysuser2@firstvm.australiaeast.cloudapp.azure.com

        // use public IP
        ssh sysuser2@20.211.21.132