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
Create copy: ```sudo dd if=/dev/sdc1 of usb.dd bs=512 count=1```\
   * if: the location of the USB drive.\
   * of: destination of copied image\
   * bs: number of bytes copied at a time\

### [2] Create hash
In order to prove that we have the unaltered copy of the device, we'll create a hash - if even one bit changes the hash will change.\

Generate hash: ```md5sum usb.dd```]

### [3] Metadata


## Tools
[usbrip](https://github.com/snovvcrash/usbrip)

## CDs

## Memory Cards

## 