# Partitioning Standard



there are 2 standard as follows:

- MBR 
  - stands for ((Master Boot Record)). 
  - this standard uses the standard BIOS partition table 
  - One advantage of GPT disks is that you can have more than four partitions on each disk. 
  - GPT is also required for disks larger than 2 terabytes (TB)
- GPT
  - stand for ((global unique identifier partition table)) .
  - this standard uses the Unified Extensible Firmware Interface (UEFI).



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



