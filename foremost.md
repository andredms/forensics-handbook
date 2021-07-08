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



