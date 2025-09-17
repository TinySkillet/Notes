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
   These  directories are where you will find your other mounted drives. It can be a floppy disk, USB stick, external hard drive, network drive or even a second hard drive.
   
   **This media folder wasn't always around though.** It was typically just mnt and that was where you mounted your storage devices. *Nowadays, most distros automatically mount devices for you, in the media directory.* 
   
