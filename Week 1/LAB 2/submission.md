1. First, create a Resource Group
        az group create --name genericRG --location australiaeast
2. Create the VM within the Resource Group

        az vm create --resource-group genericRG --name firstVM --image UbuntuLTS --admin-username sysuser2 --size Standard_B1ls --generate-ssh-keys
3. Connect via SSH

        ssh sysuser2@20.211.21.132
4. List all available images by a particular distribution eg CentOS. Remember to exit the VM first

        az vm image list --offer CentOS --all --output table
5. Create a VM based off a specific image

        az vm create --resource-group genericRG --name secondVM --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
6. List available VM sizes in a particular region/location

        az vm list-sizes --location australiaeast --output table
7. Resize an exisiting VM if the desired size is available on the current Azure cluster

        az vm resize --resource-group genericRG --name secondVM --size Standard_DS4_v2
8. Retrieve the current state for a specific VM

        az vm get-instance-view --name secondVM --resource-group genericRG --query instanceView.statuses[1] --output table
9. Get IP address of existing VM

        az vm list-ip-addresses --resource-group genericRG --name secondVM --output table
10. Stop VM

        az vm stop --resource-group genericRG --name secondVM
11. Start VM

        az vm start --resource-group genericRG --name secondVM
12. Delete VM resources

        az group delete --name genericRG --no-wait --yes