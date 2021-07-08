# Foremost

Foremost is a Linux CLI tool that attempts to recover and reconstruct files based on their headers, footers and data structures - no meta data is used. This process is known as 'file carving'. 

Foremost is able to handle many file types, including but not limited to:
* exe
* zip
* pdf

This is particuarly handy in regards to incideint response, as malicious files can be reconstructed to give us a better idea of what happened. Furthermore, it's also a handy tool to recover data that has been corrupted.

## Usage

### [1] Scan 

Foremost can be used to scan for deleted files by providing it with a source (such as a partition or .dd file - like the one generated in [devices.md](devices.md) for USBs). 

```sudo foremost -i /dev/sdb1```

This creates a folder called ```output``` which contains a subdirectory for each file type we're attempting to achieve (e.g. exe would have a folder within output). 

If we want to narrow down our search (e.g we only want to find .exe and .zip files) we can use the following flag:

```sudo foremost -t exe, zip -i /dev/sdb1```