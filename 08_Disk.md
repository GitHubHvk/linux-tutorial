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

