# Files Systems

A file system dictates how data is stored and retrieved on a device. If no file system is used, data would be placed in one large block with no way to tell apart two seperate pieces of information. Through using a file system, data can be easily isolated and identified in what are known as 'files'. 

File systems vary from device to device - this guide will cover the most relevant ones.

## NTFS (Windows)

NTFS (New Technology File System) is a process that Windows machines use to store, organise and find files on a harddrive. 

### Features 
* File compression allows for increased storage space.
* Permissions can be placed on files for greater security.
* An audit is kept of all the files added, modified or deleted on a drive. This log is kept in the Master File Table (MFT).

macOS devices can read NTFS formatted devices but can only be written to NTFS with third-party applications.

## APFS (Apple)

Prior to 2017, every apple device used the HFS+ file system - APFS (Apple File System) was developed to fix the [core problems](https://en.wikipedia.org/wiki/HFS_Plus#Criticisms) of it. 

### Features 
* Snapshots can be used to revert back to a previous copy of the device's file system - much similar to a backup. These are read-only. 
* A key can be setup to encrypt a device's entire disk. 

## HFS+ (Apple)

HFS+ (Hierarchial File System Plus) contains a volume header, startup file, attributes file, extents overflow files and a catalog file. 

### Features

### Structures
#### Volume Header

The volume header contains information about the structure of the HFS volume. A backup of this header exists and can be found in the last 1024 bytes of the volume - this is particuarly handy when it comes to disk repair if the original header is damaged.

The size of allocation blocks, a timestamp of when the volume was created and the location of other volume structures (such as the ones below) are also contained within the volume header.

#### Catalog File

The catalog file contains information about the folder and file heriarchy on a volume as well as a metadata such as when files were morified, accessed or created. 

## Ext4 (Linux)

## FAT

