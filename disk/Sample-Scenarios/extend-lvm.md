# Subject

we want to create a VM with the help of vagrant, then add a new hard disk of size 50G to the VM by the help of `Oracle VM VirtualBox Manager`, then create 2 partitions of type LVM, one with 30G size and the other with 20G of size. finally we want to extend 30G one of our existing partition, and mount another 20G to the path `/mnt`.



**Caution:** our host machine is `Windows 11`, and our gust will be `ubuntu`



***

# Setup Environment

create a directory named `vag-disk-test` and navigate to it:

```powershell
mkdir vag-disk-test
cd vag-disk-test
```



create a file named `Vagrantfile` and copy the following content to it:

```powershell
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
end
```



now execute the following command to setup and run the gust VM machine:

```powershell
vagrant up
```

 if you have not the box `hashicorp/bionic64` downloaded on your host machine, first it will be downloaded and then will be used to setup VM. 



after the VM has been setup and is run, execute the following command to connect and login to the created guest VM:

```powershell
vagrant ssh
```



execute the command `lsblk`; you will see following structure:

```powershell
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                      8:0    0   64G  0 disk
└─sda1                   8:1    0   64G  0 part
  ├─vagrant--vg-root   253:0    0   63G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
```



now execute command `poweroff` in gust VM.



**HINT:** you can download box `hashicorp/bionic64` manually by executing the following command:

```powershell
vagrant box add hashicorp/bionic64
```



***

# Add Hard Disk To VM

now open the application `Oracle VM VirtualBox Manager`; after above step, we will have a new VM that its name starts with `vag-disk-test`. select it, and then follow steps bellow:

- click on `Settings`.
- from left menu of opened dialogue select `Storage`.
- in the `Storage Devices` box select `Controller: SATA Controller`  and click on the icon that says `Adds hard Disk`.
- a dialogue is opened; click on icon that has subtitle `Create`.
- in the opened dialogue in `Virtual Hard disk file type` page, select VDI and click `Next` button.
- in the `Storage on physical hard disk`, do not select anything and click on `Next` button.
- in the `File location and size` page, Select 50 GB and enter `vag-disk-test.vdi` as the name, and click on `Finish` button. in this step the dialogue will be closed, and you will see a list of hard disks. one of them is named `vag-disk-test.vdi` which is Not Attached.
- double click on `vag-disk-test.vdi`. the dialogue will be closed, and you will see that this hard disk is added to your VM Storage. click on `Ok` button.



now navigate to the directory `vag-disk-test` and execute `vagrant up`, and after completing setup process, execute `vagrant ssh`. 

**Hint:** so that we don't have to enter `sudo` every time before our commands, execute command `sudo su`.

now execute the command `lsblk`; you will see following structure:

```powershell
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                      8:0    0   64G  0 disk
└─sda1                   8:1    0   64G  0 part
  ├─vagrant--vg-root   253:0    0   63G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                      8:16   0   50G  0 disk
```



as you can see, we have a new disk named `sdb` added that has not any partition.



***

# Create Partitions

as we want to create 2 partitions, one 30 GB and the other 20 GB, so we need to repeat following steps to times with little difference:

1. execute `fsdisk /dev/sdb`
2. type `g` to say our partition table is of type `GPT` .
3. type `n` to create our first partition.
4. click enter to set id `1` to our first partition..
5. click enter to start our first partition from the first sector of available space.
6. enter `+30G` to specify 30 GB of space.
7. repeat steps 3 to 6 to create second partition, but in step 6 enter nothing and press enter key to assign reminding of the disk space which is 20 GB to the partition.
8. type `t` to specify to change partition type.
9. type `1` to select our first partition.
10. tyle `L` to see the list of available type. in the list look for type `Linux LVM` and memorize the number associated with it. in our example this is `31`.
11. type `q` to quite type list and return to the remaining procedure.
12. type `31` and press enter key. 
13. repeat steps 8, 9, 12 to select `Linux LVM` type for partition 2.
14. finally type `w` to write the partition table to the disk.



now execute the command `lsblk`; you will see following structure:

```powershell
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                      8:0    0   64G  0 disk
└─sda1                   8:1    0   64G  0 part
  ├─vagrant--vg-root   253:0    0   93G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                      8:16   0   50G  0 disk
├─sdb1                   8:17   0   30G  0 part
└─sdb2                   8:18   0   20G  0 part
```



as you can see, our `sdb` added disk, is structured to 2 partitions. 



***

# Extend `sda1` partition

execute the the command `vgs` to see list of available virtual groups and their sizes; the result in our example is:

```powershell
  VG         #PV #LV #SN Attr   VSize   VFree
  vagrant-vg   1   2   0 wz--n- <64.00g    0
```



as it is visible, we have a volumes group named `vagrant-vg` which its size is 64 GB and has not any free volume.

on first step we need to extend this volume group by Adding physical volumes to a it, to be able to use it to cover our partition as a new member of it.

so execute the following command to add physical volume `sdb1` to the volume group `vagrant-vg`:

```
vgextend vagrant-vg /dev/sdb1
```

 

now execute the the command `vgs` again to see changes in virtual groups and their sizes; the result in our example is:

```powershell
  VG         #PV #LV #SN Attr   VSize  VFree
  vagrant-vg   2   2   0 wz--n- 93.99g <30.00g
```



as it is obvious, our volumes group named `vagrant-vg` size changed to 93.99 GB and its free size changed to 30 GB.



if you execute command `lsblk`, you will see still no changes has been made in the volume of partition `sda1`. so we need to add it to our volume group `vagrant-vg`. to do so  first we need to know the path of `sda1`; as `sda1` is divided between 2 volume groups, that  are `root` and `swap_1` (execute command `ls /dev/vagrant-vg -l` to see list), we should be aware that we want to add `sdb1` to the volume group related to `root.` so execute the following command:

```powershell
lvextend /dev/vagrant-vg/root /dev/sdb1
```



if you execute command `lsblk`, the size of partition `sdb1` which is mounted to path `/` is changed:

```powershell
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                      8:0    0   64G  0 disk
└─sda1                   8:1    0   64G  0 part
  ├─vagrant--vg-root   253:0    0   93G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                      8:16   0   50G  0 disk
├─sdb1                   8:17   0   30G  0 part
│ └─vagrant--vg-root   253:0    0   93G  0 lvm  /
└─sdb2                   8:18   0   20G  0 part
```



but if you run the command `df -h` you will see that the size of partition mounted to path `/` (in our example its name is `vagrant--vg-root`) is not changed yet. to force the changes to be reflected in partition, we should execute the following command:

```powershell
resize2fs /dev/vagrant-vg/root
```

 

**Caution:** the steps above have an issue, and that is after reboot the system does not boot. this need to be investigated and be solved.



***

# Mount `sdb2`

execute the following command to set a format for partition `sdb2`:

```powershell
mkfs.ext4 /dev/sdb2
```



execute the following command to mount it to the path `/mnt`

```powershell
mount /dev/sdb2 /mnt/
```



the above command mount our partition, but after reboot this partition will not be mounted to `/mnt`. to make this change persistent, we should write our mount at the end of the file `/etc/fstab` as follow:

```powershell
/dev/sdb2 /mnt ext4 defaults 0 0
```

 

now in order to `fstab` to be applied to the current state of the VM machine and we don't have to reboot the system, execute the following command:

```powershell
mount -a		
```



to see the result execute the command `lsblk`.