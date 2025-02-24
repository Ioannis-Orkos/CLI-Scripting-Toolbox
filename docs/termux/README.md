# Android-Termux Setup Guide

## Guide Links

> Termux wiki Tips and tricks about using Termux application and its packages [Link](https://wiki.termux.com/wiki/Main_Page).


## Introduction
   
This guide provides a list of commands and tools for Termux on an Android device, including various programming tools, SSH, VNC servers, networking utilities, AI tools, and more.


## Contents

- [Initial setup](#installation)

    - [Installation](#installation)
    - [Initial setup](#initial-setup--repository-configuration)
    - [User Management](#user-management)
    - [System tools](#system-tools)

- [Termux Tools](#system-tools)
    - [zsh and ohmyzsh setup](#zsh-and-ohmyzsh-setup)
    - [SSH](#ssh)
    - [VNC Server](#vnc-server)
    - [GUI & GNU Applications](#gui--gnu-applications)
    - [Internet & Network](#internet--network)

- [Programming tools](#programming)

    - [Apache](#apache)
    - [PHP & phpMyAdmin](#php-phpmyadmin)
    - [MySql](#mysql)
    - [NodeJS & MongoDB](#nodejs-mongodb)
    - [Java & Gradle](#java-gradle)

- [Distros & Virtualization](#other-distros--virtualization)
    - [Proot](#proot)
    - [Kali linux](#kali-linux)

- [Other scripts and projects](#other-scripts-eg-hacking--security-tools)
    - [Zhisher](#zhisher)

- [AI](#ai)
    - [llama.cpp](#llamacpp)


- [PC realated setups](#pc-related-setups)



### Installation

> Termux application can be downloaded from `F-Droid` from [here](https://f-droid.org/en/packages/com.termux/).
#



### Initial Setup & Repository Configuration
```bash
tmux-change-repo

pkg update
pkg upgrade

pkg install x11-repo
pkg install tur-repo #Contain mangodb

pkg install termux-api

termux-setup-storage #Get access to memory
```



### User Management
```bash
pkg install termux-auth

passwd  #To set password
whoami  #Display current username
```



## zsh and ohmyzsh setup
```bash
pkg install zsh
chsh -s zsh  # Change shell to Zsh
termux-reload-settings
```
> To setup omz. Click [here](support/omzConfigREADME.md).
#




### System tools
```bash
pkg i htop #Task mananger

#contains a set of scripts for controlling services.
pkg install termux-services
```
<details>
<summary> More Ram if needed </summary>

```bash
termux-setup-storage
dd if=/dev/zero of=$HOME/swapfile bs=1M count=4048
mkswap $HOME/swapfile
swapon $HOME/swapfile
```
</details>



### SSH
> port 8022
```bash

pkg install openssh

sshd  # Start SSH server
pkill sshd  # Stop SSH server
```

<details>
<summary> Basic SSH Usage  </summary>

<br>

```bash
ssh -p 8022 u0_a472@192.168.158.147  # Connect via SSH
ssh -p 8022 -L 5000:localhost:8080 -N u0_a472@192.168.231.118  # Create SSH tunnel

ssh-keygen -t rsa -b 2048 -f id_rsa  # Generate SSH key
```

</details>

<br>

> For detailed guide. Click [here](../cli_tools/sshREADME.md).
#





### VNC Server
>port 5901

```sh

pkg install tigervnc

vncserver  # Start VNC Server
vncserver -list  # List active sessions
vncserver -kill :1  # Stop VNC Server

```



### GUI & GNU Applications
```sh
pkg install xfce 
pkg install xfce4*

pkg install netsurf
pkg install gimp
pkg install firefox

vncserver :1  # Start vnc server on export DISPLAY=:1
xfce4-session --display=:1  # Start XFCE Session
```




### Internet & Networking

### Git and github
```bash
pkg install git
```
> For detailed guide. Click [here](../cli_tools/gitREADME.md).
#

### nmap
```bash
pkg install nmap
```
> For detailed guide. Click [here](../cli_tools/nmapREADME.md).
#

### Others
```bash
pkg install wget
pkg install curl

#check global ip address
curl -4 ifconfig.me
wget -qO- ifconfig.me
wget -qO- -4 ifconfig.me
```


```bash
pkg install tor
pkg install proxychains-ng

#To link tor with proxychain
proxychains4 --config usr/etc
# add -> socks5 127.0.0.1 9050
# add -> socks4 127.0.0.1 9050

#start tor
tor

#use
proxychains4 nmap google.com
proxychains4 netsurf
proxychains4 firefox
```


```bash
#Ngrok
#download ngrok file
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-arm64.tgz && tar -xzvf ngrok-v3-stable-linux-arm64.tgz 

./ngrok config add-authtoken <token here>


#use
ngrok http 80

#To tunnel create connection
ngrok tcp 8022  
#To connect
ssh u0_a472@2.tcp.eu.ngrok.io -p 11049
ssh  -L 5900:localhost:5901 -N u0_a472@2.tcpeu.ngrok.io -p 11634
```


```bash
#Cloudflare
#https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm' 

pkg install cloudflared

#use
cloudflared tunnel -url localhost:8080
cloudflared tunnel --url localhost:8080 --protocol http2
```

```bash
#localxpos
loclx account login
loclx tunnel http
loclx tunnel http --to :80
```

```sh
#Tmate - utility for sharing terminals
pkg install tmate
tmate -F
```



## Programming 

### Apache
> port 8080
```bash
pkg install apache2
#Start server apache
httpd or apachectr start stop
killall httpd


#where   /data/data/com.termux/files/usr/etc/apache2
LoadModule php_module libexec/apache2/libphp.so

LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so
#LoadModule mpm_worker_module libexec/apache2/mod_mpm_worker.so
LoadModule authn_file_module libexec/apache2/mod_authn_file.so
<FilesMatch \.php?>
          SetHandler application/x-httpd-php
</FilesMatch>

<IfModule dir_module>
    DirectoryIndex index.html  index.php
</IfModule>

#optional for default workstaioin
DocumentRoot "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs"
<Directory "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs">
#data/data/com.termux/files/home/storage/Shared/A-Drive/Project

```





### php phpMyAdmin

```bash
pkg install php  php-apache
pkg install libsodium php-sodium #library for phpmyadmin

#pkg install phpmyadmin # Preferably download version 6.xx for the website.


nano /data/data/com.termux/files/usr/share/phpmyadmin/config.inc.php

$cfg['Servers'][$i]['host'] = '127.0.0.1;
$cfg['Servers'][$i]['AllowNoPassword'] = true;

#use
php -S localhost:8001 -t $PREFIX/share/phpmyadmin/
```



### mysql
> port 3306
```bash
pkg install mariadb
##setup
mariadb-install-db

#use with out password
mariadbd-safe -u root

##to add pasword
mysql -u root
#then in mysql  area
use mysql;
set password for 'root'@'localhost' = password('root');
flush privileges;
quit;
##

mysql -u root -p


Start mysql server
mysqld
background start
mysqld_safe &
mysqld_safe -u root

mysql -h localhost
```


### MongoDb
```bash
pkg i tur-repo
pkg i mongodb

#use
mongod #server
mongo 
```


### NodeJs
```bash
pkg install nodejs

# will also download npm with nodejs
npm
```


### Java Gradle
```bash
pkg install openjdk-17
pkg install gradle
```



## Other Distros & Virtualization 

### Proot
```bash
# To check ariticuture
uname -m      

pkg install proot-distro

#use
#alpine
proot-distro install alpine
proot-distro login alpine
```


### Kali linux
```bash
	-> apt update
	-> apt upgrade	
	-> termux-setup-storage
	-> pkg install fish
	-> pkg install wget
	-> wget -O install-nethunter-termux https://offs.ec/2MceZWr
	-> ls 
		check for install-nethunter-termux or -> ls -l to check size
	-> chmod 777 install-nethunter-termux
	-> ./install-nethunter-termux  
	
	   after kali start	
	-> nethunter kex passwd
	-> nethunter kex &
```





## Other scripts Eg.. hacking & Security Tools

### Zhisher
```sh
git clone https://github.com/htr-tech/zphisher
cd zphisher
chmod +x zphisher.sh
./zphisher.sh
```
#


# AI

## llama.cpp


 ### Links 

   > https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md <br>
   > https://developer.android.com/ndk/downloads <br>
   > https://cmake.org/download/ <br>
   > https://github.com/skeeto/w64devkit/releases <br>



### Bulid llama.cpp on a PC for termux
#
### Method one
#### Tools
   > w64devkit <br>
   > android-ndk <br>
   > cmake <br>
#### 
```cmd
git clone https://github.com/ggerganov/llama.cpp

cd llama.cpp

mkdir build-android

cd build-android

export NDK=D:/AI/Chat-AI/llama/Android/android-ndk-r27

export cmake=D:/AI/Chat-AI/llama/Android/cmake-3.29.3-windows-x86_64/bin/cmake.exe

$cmake  -G "Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE=$NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-23 -DCMAKE_C_FLAGS=-march=armv8.4a+dotprod ..

make
```
#



## Bulild llama.cpp on Termux
### Method one
#### Tools
   > android-ndk-r26b for aarch64
#### 
#### Links
   > [android-ndk-r26b for aarch64](https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md) <br>
   > https://github.com/Lzhiyong/termux-ndk/releases 
#### 
#### Build Packages
```bash
pkg update && apt upgrade -y
pkg  install git make cmake 
pkg  install wget unzip
```
#### Build code
```bash
cd /data/data/com.termux/files/home #or cd ~

mkdir .devkit

cd .devkit

wget https://github.com/lzhiyong/termux-ndk/releases/download/android-ndk/android-ndk-r26b-aarch64.zip

unzip android-ndk-r26b-aarch64.zip

export NDK=/data/data/com.termux/files/home/.devkit/android-ndk-r26b

git clone https://github.com/ggerganov/llama.cpp

cd llama.cpp

mkdir build-android

cd build-android

cmake -DCMAKE_TOOLCHAIN_FILE=$NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-23 -DCMAKE_C_FLAGS=-march=armv8.4a+dotprod ..

make
```
#

#### Usage
```bash

#lib FILES
#copy lib files form build forlder to AI/lib
libggml.so , libllama.so , libomp.so

export LD_LIBRARY_PATH=/data/data/com.termux/files/home/AI/lib
llama-cli -m your_model.gguf --interactive-first -cnv
llama-server -m your_model.gguf -c 2048
##


cd  AI/llama.cpp/build-android/bin
./main --interactive-first -m ./../../../Models/dolphin-2.6-mistral-7b.Q4_0.gguf -t 8
./main --interactive-first -m ./../../../Models/dolphin-2.6-mistral-7b.Q2_K.gguf -t 8
./server -m d -m ./../../../Models/dolphin-2.6-mistral-7b.Q4_0.gguf

./chat --interactive-first -m Models/llama-2-7b.Q2_K.gguf
./server -m models/dolphin-2.5-mixtral-8x7b.Q5_K_M.gguf -c 16384

##You can run a basic completion using this command:
llama-cli -m Models/your_model.gguf -p "I believe the meaning of life is" -n 128

##conversation mode by passing -cnv as a parameter:
llama-cli -m your_model.gguf -p "You are a helpful assistant" -cnv

##fast old 
./chat -m Models/dolphin-2.6-mistral-7b.Q2_K.gguf  --interactive-first -n 128 -p "give me templite email to quit work"

##HTTP server
./llama-server -m your_model.gguf --port 8080
```


## PC related setups

### For issuse with android cuting off 
```cmd
root: su -c "/system/bin/device_config set_sync_disabled_for_tests persistent; /system/bin/device_config put activity_manager max_phantom_processes 2147483647"
adb: adb shell "/system/bin/device_config set_sync_disabled_for_tests persistent; /system/bin/device_config put activity_manager max_phantom_processes 2147483647"
```
### Too run termux on a docker
```cmd
// Docker Termux on PC UNTESTED
// https://github.com/termux/termux-docker
docker run -it termux/termux-docker:latest
```
##