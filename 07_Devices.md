# Identify a Device and its Permissions

to see a device and its permissions run the command (( **ls -l** )) in the directory you desire. this command will list all files and devices as following example:

```powershell
brw-rw---- 1 root disk 8, 1 Sep 6 08:37 sda1
crw-rw-rw- 1 root root 1, 3 Sep 6 08:37 null
prw-r--r-- 1 root root 0 Mar 3 19:17 fdata
srw-rw-rw- 1 root root 0 Dec 18 07:43 log  
```

 

**Caution:** note the first character of each line, if it is b, c, p, or s, the file is a device.

- ##### b

  - is a block device. disk devices are from this type. in the result of ((ls -l)) command the numbers before date are major  and minor device numbers that kernel uses to identify device.

- ##### c

  - character device. printers are from this type. they do not have a size. kernel can not back up data stream after it has passed data to the device or process. in the result of ((ls -l)) command the numbers before date are major  and minor  device numbers that kernel uses to identify device.

- ##### p

  - pipe device. named pipes are like character devices, with another process at the end of the I/O stream instead of a kernel driver. 

- ##### s

  - socket device. are interfaces for interprocess communication.



***

# udevadm

this command reveals more info about the device. for example following command shows more detailed info about device ((/dev/sda)):

- udevadm info --query=all --name=/dev/sda



***

# dd

the sole purpose of this program is to read input from a file or a stream and write it to the output of a file or stream.

extremely useful when working with block and character devices.

with respect to block devices it could process a chunk of data in the middle of a file.



**example:** 

- dd if=/dev/zero of=/var/my_dir/new_file.txt bs=1024 count=1
  - this command write first (count=1, number of blocks) 1024 bytes (bs=1024, also it could be bs=1k or bs=2b as k is equal to 1024 bytes and b is equal to 512 bytes) of stream from input device /dev/zero (a continuous stream of zeros) as ((if)) to a new file named new_file.txt in the path /var/my_dir as ((of)).
  - if input and output block size are equal we can use ((bs)), otherwise we have two options as ((ibs)) for input block size and ((obs)) for output block size. 



***

# List b and c Devices

- ###### cat /proc/devices

  - this command lists block and character devices.



***

# lsscsi

this program is used to list devices that communicate with kernel based on SCSI (small computer system interface) protocol.



to show generic devices list use option ((-g)) as follow:

- lsscsi -g





**Caution:** if command is not found, install the program by running command ((apt install lsscsi)).



***

# Serial Ports

port known in windows ad COM0, COM1 are /dev/ttyS0, /dev/ttyS1 and so on, in Linux.



***

# Create Device File

following command creates a block device named ((sda3)) in path ((/dev)) with major number 8 and minor number 1:

- mknod /dev/sda3 b 8 3



***

# Disks link file

to see the link files of the disks in Linux, run the following command:

- ls -l /dev/disk/by-id

***

# Monitor udev

to monitor the events that udev program receives from kernel and sends out after rule processing, and kernel events run following commands:

- udevadm monitor  

to only see kernel events use option ((--kernel)), and to see only udev events use ((--udev)) option in the command as follow:

- udevadm monitor --kernel
- udevadm monitor --udev



to filter the events for example pertaining to changes in ((scsi)) subsystem, use the option ((-subsystem-match)) as follow:

- udevadm monitor --subsystem-match=scsi



***

# USB bus

to list USB buses and devices connected to the in Linux, use the following command:

- lsusb
- lsusb -v
  - this command show the information in verbose mode and also shows more detail.
- lsusb -s 1
  - this command display devices with bus number 1.
- lsusb -s 2:1
  - this command display devices with bus number 2 and device number 1.
- lsusb -t
  - shows info in tree structure.
- lsusb -D /path/to/device/file
  - show information about the specified device.
