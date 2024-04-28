Life in DROPBOX

Run this to change vm settings (mem/cpu)
vagrant reload --provision 

Softer settings like mount points:
vagrant reload


After initial 'vagrant up' and first reboot from inline
you can access in to system with 'vagrant ssh'
vagrant@xr-d:~$ 

Go now: cd /vagrant_data

agrant@xr-d:/vagrant_data$

Then start kali
docker run -it -v $(pwd):/root/shared kalilinux/kali-rolling /bin/bash
then do cd /root/shared and cat /etc/os-release > kali_info.txt
and you see same file in your dropbox folder still


┌──(root㉿b39282c8ab69)-[/]
└─# cd /root/shared/

┌──(root㉿b39282c8ab69)-[~/shared]
└─# cat /etc/os-release > kali_info.txt

┌──(root㉿b39282c8ab69)-[~/shared]
└─# 


so what about...secret file system? piece of cake
sudo apt-get install encfs in main vagrant box


mkdir /vagrant_data/secret_data
mkdir /home/vagrant/secret_data


encfs --public /vagrant_data/secret_data /home/vagrant/secret_data

vagrant@xr-d:~$ encfs --public /vagrant_data/secret_data /home/vagrant/secret_data
option '--public' ignored for non-root userCreating new encrypted volume.
Please choose from one of the following options:
 enter "x" for expert configuration mode,
 enter "p" for pre-configured paranoia mode,
 anything else, or an empty line will select standard mode.
?> p



New Encfs Password:

lets use password MegaMoth4

next time you mount it will just ask password

once mounted
vagrant@xr-d:~$ echo mysecret > /home/vagrant/secret_data/test.txt
and lets see whats in dropbox folder

vagrant@xr-d:~$ ls -a /vagrant_data/secret_data/
.  ..  b4jwfk,46KjWHVzI0qhVR8O8  .encfs6.xml
vagrant@xr-d:~$

back in host machine, or windows in my case, lets push that secret data to git... as its already in dropbox.... but in case you need


C:\Dropbox\Infra\x-rd>git add secret_data

C:\Dropbox\Infra\x-rd>git commit -m "encryption layer"
[main 9d5c508] encryption layer
 2 files changed, 38 insertions(+)
 create mode 100644 secret_data/.encfs6.xml
 create mode 100644 secret_data/b4jwfk,46KjWHVzI0qhVR8O8

C:\Dropbox\Infra\x-rd>git push
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 991 bytes | 991.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:tofubotu/xr-d.git
   4512993..9d5c508  main -> main


And this is how you have your encryption layer and vagrant box with docker on top of dropbox folder...
you are now hardware independent ....

// Tofu
