# Partitioning Standard



there are 2 standard as follows:

- MBR 
  - stands for ((Master Boot Record)). 
  - this standard uses the standard BIOS partition table 
  - it contains three types of subdivisions:
    - primary: each MBR disk has limitation of 4 primary disk. if more than 4 is needed, one partition should be designated as extended. 
    - extended: it breaks down to logical partition.
    - logical
  - to view MBR table run command ((journalctl -k)).
- GPT
  - stand for ((global unique identifier partition table)) .
  - this standard uses the Unified Extensible Firmware Interface (UEFI).
  - One advantage of GPT disks is that you can have more than four partitions on each disk. 
  - GPT is also required for disks larger than 2 terabytes (TB)



***

# Tools

there some tools to work with partitions, that some of the most important ones are as follow:

- parted
  - this is a text based program, that supports both MBR and GPT.
- gparted
  - this is the graphical version of parted.
  - to install it, use command ((apt install gparted)).
- fdisk
  - this is the traditional text based program. older version was limited to MBR, but recent ones support new standards including GPT.



***

# View Partition Table

following commands shows the system partition table:

- parted -l
- fdisk -l



***

# Partition Table Reload

we can make changes in partition table with each of programs ((fdisk)) and ((parted)); but changes in made by ((fdisk)) is not committed immediately, and it is needed to finalize it by exiting the program. 

in order to reload new changes of partitions in disk ((/dev/sda)) made by ((fdisk)) program, you can run the following command:

- blockdev --rereadpt /dev/sda



***

# lsblk

this command shows the device block location structure.



***

# Partitioning Disk

to partition a disk we can use program ((fdisk)). say we have attached a new 1 GB of disk to the system, and want to partition it to 2 MBR primary parts respectively 200MB and 800MB. we should do the following steps:

- lsblk
  - this command lists the structure of device locations. we run it to find what is our new attached disk. say for example it is ((/dev/sdb)) .
- fdisk /dev/sdb
  - this command starts the steps of creating partitions by prompting you to enter a letter that specifies your action.
  - for learning the available actions, press m to list them; some of the important items are as follows:
    - p: shows the status of disk current partitions.
    - d: delete the selected partition.
    - n: create a new partition. after entering ((n)) you will prompt to select new partition characteristics.
    - w: write the changes to the partition table.
- enter p, if there is any existing partition, delete them with ((d)).
- enter n
  - it will ask following questions:
    - select partition type: ((p)) for primary and ((e)) for extended. enter ((p)).
    - select the number of partition. press enter to set the incremented number started from 1.
    - select the starting sector. press enter.
    - select the end of partition. enter ((+200MB)).

- enter n to create second partition, this time for all questions just enter and don't type anything.
- enter ((p)) to see the setting of new created partitions.
- press ((w)) to write changes to the partition table.



***

# Partition Filesystem

to see the filesystem of system partitions, run the following command:

- lsblk -f

to change the filesystem of a partition, say ((/dev/sda2)) run following command:

- mkfs -t ext4 /dev/sda2

to list all possible filesystem, run the following command:

- ls -l /sbin/mkfs.* 