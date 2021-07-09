# iOS Devices
Much like USBs, mobile phones are extremely portable and contain a treasure trove of information. Unlike Linux and Windows, Apple uses the HFS+ file system (or more recently APFS). 

Passcode locked devices are a challenge as a passocde is required in order to acquire data. When performing a forensics investigation on an iOS device, 'airplane mode' should be used in order to prevent a third party from remotely accessing the device and tampering with it once it's been sized. 

## Partitions 

iOS devices contain two partitions - the firmware and user data partition. The firmware partition is readonly unless an update is being performed and contains no user data. The user data partition contains everything related to the user. 

## Acquiring Data

### iTunes Backup

If an iOS device is not physically available, it is possible to retrieve the latest back up through a workstation (such as a laptop or desktop) that the device previously connected to. iTunes performs an automatic backup during the syncing process or whenever an upgrade is performed. These backups can be found in the following locations:

* Windows 7 - 11: ```%systempartition%\Users\%username%\AppData\Roaming\Apple Computer\MobileSnc\Backup```
* macOS: ```Users/%username%/Library/Application Support/MobileSync/Backup```

Multiple files are contained within the above directories:

* Status.plist provides information about the latest backup.
* Info.plist contains information that can be used to confirm the backup matches the device.
* IMEI number (a unique 15-digit number every mobile device has).
* Phone number.

The file names within are convereted to a SHA1 string of the original name. Files ending with *.mddata and *.mdinfo are the most interesting as they contain user data. 

### Logical Methods

The most popular approach today is to use the logical acquisition method - where one recovers the active files on an iOS device using a syncrhonisation method built into iOS. In doing so, one can gather SMS texts, call logs, calendar events, contacts, photos, web history and e-mail accounts. 

[iExplorer](https://macroplant.com/iexplorer) can be used in order to quickly export call history, texts, photos, contacts and bookmarks. Information such as date modified is avaiable and it's even possible to recover data from after a factory reset. 

### Physical Methods

Tools can be used to extract an image of an iOS device if it is physically available:

* [Lantern 2](http://www.mobileforensicscentral.com/mfc/products/lantern.asp?pg=d&prid=387&pid=]) is able to extract data from any iOS device running any version.
* [iXAM](https://wikileaks.org/spyfiles/docs/FORENSICTELECOMMUNICATIONS-2011-iXAMZeroFore-en.pdf) has the ability to retrieve all different types of data - potentially even all voicemails recieved over the lifetime of a handset. Other information such as GPS locations, bluteooth pairings, wireless network details, internet history, text messages and content downloads can also be recovered.

### Jailbreaking 

If none of the above methods work, one can jailbreak a device to replace the firmware partition with a hacked version that allows the installation of tools not normally available. These include services such as SSH and applications like Terminal. 

Once jailbroken (using a tool such as redSn0w), a forensic workstation is typically setup on the same wireless network as the iOS device and the following command can be run to extract an image:

```ssh root@172.16.103.106 dd if=/dev/rdisk0 bs=1M | dd of=ios-root.img```

Unfortunately, this image is encrypted and one must obtain the password from the device owner in order to decrypt it. Currently, [checkra1n](https://checkra.in/) can be used to bypass such restrictions. checkra1n can be installed on a locked device in DFU mode with an unknown password to then extract information through the [checkm8](https://checkm8.info/) vulnerability. 

* [1] Install latest release of checkra1n
* [2] Connect the device and put it in [DFU mode](https://www.theiphonewiki.com/wiki/DFU_Mode)
* [3] Open terminal and type: ```cd /checkra1n.app/Contents/MacOS/``` and ```./checkra1n_gui -```
* [4] ```sudo iproxy <local_port> 44```
* [5] Download directories (listed below): ```sshpass -p alpine scp -P <Local_Port> -rp root@localhost:/path_to_folder /path_to_folder```

Here's a list of interesting files:

![](https://i.imgur.com/B9xkB1B.png)
  
## Evidence 

An iOS device will contain many SQLite and plist files that can be used to build up a case. The directory structure of all iOS devices resembles that of a UNIX system. Many files are stored in .txt format, others are in SQLite databse, XML or binary format. 

Default applications (such as Contacts) store their data in ```private/var/mobile/Library```. Downloaded applications (such as Spotify) store their data in ```private/var/mobile/Applications```. 


### Downloaded Applications 

```private/var/mobile/Applications```

When an application is downloaded, a new directory is created within the ```mobile/Application``` folder. info.plist is included in all application directories and application specific usernames, passwords, cookies and images can also be found here.

### Images

```private/var/mobile/media/DCIM```

Images are located within ```private/var/mobile/media/DCIM```. Photos within the ```100APPLE``` directory indiciate that the photo was taken on the device itself - sequential numbering is used to show the order in which the photos were taken, e.g. IMG_0001. If there is a number missing in the sequence of photo files, one can assume that the photo was deleted. 

Screenshots are located within the ```999Apple``` folder - if an illegal application was used, any screenshots that the user took of it may be here if they have not deleted them.

### Keystrokes

```private/var/mobile/Library/Keyboard```

A list of all words typed by the user throughout the lifetime of a device can be found in ```dynamic-text.dat``` within ```private/var/mobile/Library/Keyboard```. Any words entered into applications such as Notes, Safari, Facebook ect. will be stored here. ```UserDictionary.sqlite``` contains a user's manual autocorrections.

### Passwords

```/private/var/Keychains```

Many iOS applications use Apple's keychain for password management. The ```key-chain-2.db``` file contains various tables such as cert, genp, inet and keys that may have account names and passwords a device has used. 

Voicemail, wireless access point and device login passwords can be found within this database. In most cases, the passwords will be encrypted by the [iOS encryption keychain procedure](https://support.apple.com/en-au/guide/security/secb0694df1a/web). 

### Notes

```/private/var/mobile/Library/Notes```

Data from the default Notes application can be found within ```notes.sqlite``` - the table 'ZNote' contains multiple columns that may be of interest:

* CREATIONDATE
* MODIFICATIONDATE
* ZTITLE

The 'ZNOTEBODY' table contains the contents of the note in the 'ZCONTENT' column.

### Texts 

```/private/var/mobile/Library/SMS```
  
Text messages are located within the ```sms.db``` - this houses both existing and deleted conversations. The tables 'message' and 'msq_pieces' will contain the most interesting information. The message table contains a row for each message - each row contains the following information:

* Recieving phone number.
* Message body.
* 'flags' which tell if the message was sent (3) or recieved (2).
* 'read' which tells if the message was read (1).

The 'msg_pieces' table contains a row for each message that contains content (e.g. a photo or video). The column 'message_id' will correspond to the rowID of the message table.

### Cookies

```/private/var/mobile/Library```

Cookies can show an investigator web browsing activity - these are stored within ```cookies.binarycookies```. A HEX editor must be used to read this file.

### Browsing Activity

```/private/var/mobile/Library/Caches/Safari```

Browsing activity is located within ```RecentSearches.plist``` and come in XML format. 

### Personal Contacts

```/private/var/mobile/Library/AddressBook```

A device owner's contacts can be found within ```AddressBook.sqlitedb```. The 'ABPerson' table contains the first and last name, birthday, job, title, nickname ect. The 'ABMultiValue' contains the column 'value' which has the e-mail or phone number of the individual in 'ABPerson'. 

### Call History

```/private/var/Library/CallHistory```

The call history of an iOS device is located within ```call_history.db```. The 'call' table contains the phone number, date and duration of the call. The 'flags' column in this table indicates whether it was an incoming (4) or outgoing (5) call.

### Geographical Location

```/private/var/Library/Caches/locationd```

Applications such as Camera often store the longitude and latitude of where a photo was taken. The ```consolidated.db``` file contains geolocation data for every cell tower the iOS device utilises. Tools such as [iPhone Tracker](peterwarden.github.com/iPhoneTracker) pull the aforementioned file and provide a graphical display of where the iOS device has been. 

## Tools
[Scalpel](https://github.com/sleuthkit/scalpel)
[Paraben Device Seizure](https://paraben.com/paraben-for-mobile-forensics/)\
[EnCase](https://security.opentext.com/encase-mobile-investigator)
[Mobile Sync Browser](http://mobilesyncbrowser.com/)
