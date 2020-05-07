# Vagrant Hybrid LAB on Virtualbox


This Vagrantfile make an hybrid lab on virtualbox with Linux, Mac and Windows Machines  

* Default Configuration requires 13GB of free RAM  
* Increase or decrease RAM configuration of any machine if necessary  
  * 4GB of RAM its the minimal required for Windows and MacOSx 
* Dont forget to create the entries on hosts file
  * Linux
    * `/etc/hosts`
  * MacOSx
    * `/etc/hosts`
  * Windows
    * `C:\Windows\system32\drivers\etc\hosts`
* Your Host machine should have NFS Server installed
  * Ubuntu 
    * `sudo apt-get update ; sudo apt-get install nfs-server nfs-common -y`
* On MacOSx step to configure `nfs-server`, you'll be asked for `sudo` password 