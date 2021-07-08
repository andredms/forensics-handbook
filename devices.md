# Desktops

# Laptops

# Smart Phones

# Tablets

# USBs
USBs (Universal Serial Bus) are small storage devices that can store malicious programs and malware such as packet sniffers and key loggers. USB forensics can be used to recover deleted data in the event of an incident. There are many creative ways in which they can be [disguised](https://www.hongkiat.com/blog/weird-and-unusual-usb-products/).

## Common Sizes
2gb - 128gb

## Recovering data
### [1] Create a copy

List drives: ```sudo fdisk -l```\
Create copy: ```sudo dd if=/dev/sdc1 of usb.dd bs=512 count=1```
* if: the location of the USB drive.
* of: destination of copied image
* bs: number of bytes copied at a time

### [2] Create hash
In order to prove that we have the unaltered copy of the device, we'll create a hash - if even one bit changes the hash will change too.\

Generate hash: ```md5sum usb.dd```

### [3] Metadata

We can look into more specific information about the copied USB device. 

File system and drive geometry: ```file usb.dd```
NTFS boot sector information: ```minfo -i usb.dd```
Allocation structures: ```fstat usb.dd```

### [4] Deleted Files

We can find information about recently deleted files on the device.

```fls -rp -f fat32 usb.dd```

* -p displays the full path of every file recovered
* -r displays the paths and folders recursively
* -f displays the file system used (FAT16, FA32)

Deleted files that have been recovered are notated with an asterisk (```*```).

### [5] Timeline

We can build a timeline analysis of our copied image. 

Prime the data: ```fls -m / -rp -f fat32 ok.dd > usb.fls```\
Convert the file from previous command: ```cat usb.fls > usb.mac```\
Make human readable: ```mactime -b usb.msc > usb.mactime```\
Read the file: ```cat usb.mactime```\

## Tools
[usbrip](https://github.com/snovvcrash/usbrip)\
[Foremost](https://linux.die.net/man/1/foremost)\
[Autopsy](https://www.sleuthkit.org/autopsy/)\
[FTK Imager](https://www.exterro.com/ftk-imager)

## CDs

## Memory Cards

## 