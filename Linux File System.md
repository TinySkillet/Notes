**Linux, following Unix, has a standard where everything is a file. **

**'/'** is the main directory that contains everything in Linux. 


#### 1. bin
   bin contains the basic binaries, which are basically applications. *ls, cat* are actually stored here. 

#### 2. sbin
   sbin contains the system binaries, that a system admin would use. Standard user wouldn't have access to these.

  Both of these folders contain the files that need to accessible when running in*single user mode.* **Single user mode** is a special mode that boots you in as a root user to allow you to do system repairs and upgrades or testing.
 
#### 3. boot
   boot contains everything your system needs to boot. The bootloader lives here. *You don't want to touch this folder.*

#### 4. dev
   This is where your devices live. Here you will find your hardware. A disk for example will be: `dev/sda` and the partitions on that disk would be `dev/sda1`, `dev/sda2`, etc.
   
   You can find anything here from your webcam to your keyboard. *This is typically an area that applications and drivers will access and is rarely something a user should be dabbling in.*

#### 4. etc
   It has been confirmed by Dennis Ritchie that *etc* actually means Et cetera. This folder is where all your **system-wide configurations** are stored. 

#### 5. lib folders (lib, lib32, lib64)
   These are where the libraries are stored. Libraries are files that applications can use to perform various functions. *Binaries in bin and sbin can may require libraries that they will reference from the lib folders.* 

#### 6. mnt, media
   These directories are where you will find your other mounted drives. It can be a floppy disk, USB stick, external hard drive, network drive or even a second hard drive.
   
   **This media folder wasn't always around though.** There used to be just **mnt** and that was where you mounted your storage devices. *Nowadays, most distros automatically mount devices for you, in the media directory.* 
   

> [!NOTE] Info
> The USB stick that you insert would be in `media/your-username/device-name`


> [!NOTE] Which directory to use?
> If you are mounting things manually, use the mnt directory. Leave the media directory to the OS to manage.

   
#### 7. opt
**opt** is usually where manually installed software from vendors reside. 

#### 8. proc
**proc** is where you will find psuedo files that contain information about system processes and resources. Every process will have a directory here which contains information on that process. 

You can  find information for CPU for example `/proc/cpuinfo`, or uptime `/proc/uptime`. 

> [!NOTE] Example
>  If you navigate to a process, say `/proc/2244` , you will see all kinds of pseudo files. This is much like in the `dev` where they are not exactly files in the system. This is the kernel translating other information to appear as files.


#### 9. root
**root** is the root user's home folder. Unlike a user's home folder, it does not contain the typical directories inside and it doesn't reside in the home directory. You can store files here if you like but you need root permission to access it.

The location of this directory ensures that root always has access to its home folder in case you have the regular users home directory stored on another drive which you cannot access.

#### 10. run
**run** is a `tempfs` file system which means it runs in RAM. Everything in it is gone when system is rebooted or shut down.  It's used for processes that start early in the boot procedure to store runtime information that they use to function.

#### 11. srv
**srv** is the service directory where service data is stored. If you run a `HTTP` or a `FTP` server, you will store the files that will be accessed by external users here.

#### 12. sys
**sys** is the way to interact with the kernel. This directory is similar to run directory. It is similar to the run directory and it's not physically written to the disk. It's created everytime the system boots up.

#### 13. temp
This is where files are temporarily stored by applications that can be used during a session.


> [!NOTE] Example
> If you are writing a document in a word processor, it will regularly save a temporary copy of what you have written here. So that if the application crashes, it can look here to see if there is a recent saved copy that you can recover.

This folder is usually emptied when you reboot the system. On occasion you may find some files or directories that remain and stuck there because the system can't delete them.
This is fine unless there are hundreds of file or are taking a lot of space. In that case you can login as the root user and manually delete them.

#### 14. usr
**usr** stands for 'Unix System Resource'.

This is the user application space where applications will be installed that are used by the user. As oppose to the bin directory used by the system and admin to perform maintenance. Any applications installed here are considered non-essential for basic system operations. 


#### 15. var
 It contains files and directories that are expected to grow in size. 


> [!NOTE] Example
> `/var/crash` contains information about processes that have crashed. `/var/log` contains log files for both system and many different application which will constantly grow in size as you use the system.

  
