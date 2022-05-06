1. Default Azure disks:
    - Operating system disk
    - Temporary disk

2. Azure data disks:
    Azure data disks provide additional durable and responsive data storage for VMs

3. VM disk types:
    - Standard disks uses HDDs and cost-effective
    - Premium disks are SSD-based, high performance low-latency disk

4. Attach a disk at VM creation:

        az vm create --resource-group genericRG --name thirdVM --image UbuntuLTS --admin-username sysuser2 --size Standard_B1ls --generate-ssh-keys --data-disk-sizes-gb 128 128

5. Attach a disk to existing VM:

        az vm disk attach --resource-group genericRG --vm-name firstVM --name myDataDisk --size-gb 128 --sku Premium_LRS --new

6. Prepare data disk:

        ssh sysuser2@20.211.21.132
        sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
        sudo mkfs.xfs /dev/sdc1
        sudo partprobe /dev/sdc1
        sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
        sudo -i blkid // save UUID of drive
        
        sudo nano /etc/fstab
        // Add a line to the fstab document. Tabs are important
        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  xfs    defaults,nofail   1  2

7. Create snapshot:

        // Get disk ID
        osdiskid=$(az vm show -g genericRG -n firstVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)

        // Create snapshot
        az snapshot create --resource-group genericRG --source "$osdiskid" --name osDisk-backup

8. Recreate disk from snapshot:

        az disk create --resource-group genericRG --name snapShotDisk --source osDisk-backup