1. First step was to list all available location codes so I could find the nearest one

        az account list-locations
2. Next was to create a Resource Group in my chosen location (South Africa)

        az group create --name genericRG --location australiaeast
3. Create the VM within the Resource Group

        az vm create --resource-group genericRG --name firstVM --image UbuntuLTS --admin-username sysuser2 --size Standard_B1ls --generate-ssh-keys
4. Open port 80

        az vm open-port --port 80 --resource-group genericRG --name firstVM
5. Connect via SSH

        ssh sysuser2@20.211.21.132
6. Install NGINX

        sudo apt-get -y update
        sudo apt-get -y install nginx
7. Delete Resource Group

        az group delete --name genericRG