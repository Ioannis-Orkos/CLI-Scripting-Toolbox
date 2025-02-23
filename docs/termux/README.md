# Android-Termux Setup Guide

## Guide Links

> Termux wiki Tips and tricks about using Termux application and its packages [Link](https://wiki.termux.com/wiki/Main_Page) .


## Introduction

This guide provides a list of commands and installations required to set up Termux on an Android device, including package installations, SSH setup, VNC server, networking tools, and AI tools etc ...



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

- [AI](#)

- [Distros & Virtualization](#distros--virtualization)

    - [Distros & Virtualization](#distros--virtualization)

- [Other scripts and projects](#hacking--security-tools)
    - [Hacking & Security Tools](#hacking--security-tools)



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


### System tools
```bash

pkg i htop #Task mananger

#contains a set of scripts for controlling services.
pkg install termux-services

```




### SSH

> port 8022

```bash

pkg install openssh

sshd  # Start SSH server
pkill sshd  # Stop SSH server

```
<details>

<summary> SSH Usage  </summary>
<br>

```bash

ssh -p 8022 u0_a472@192.168.158.147  # Connect via SSH
ssh -p 8022 -L 5000:localhost:8080 -N u0_a472@192.168.231.118  # Create SSH tunnel

ssh-keygen -t rsa -b 2048 -f id_rsa  # Generate SSH key

```

</details>





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




### Internet & Network

```sh
pkg install git
pkg install wget
pkg install curl

pkg install nmap


#check global ip address
curl -4 ifconfig.me
wget -qO- ifconfig.me
wget -qO- -4 ifconfig.me
```

```sh
pkg install tor
pkg install proxychains-ng

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


```sh
#Ngrok
#download ngrok file
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-arm64.tgz && tar -xzvf ngrok-v3-stable-linux-arm64.tgz && ./ngrok

./ngrok config add-authtoken <token here>

ngrok http 80

#To tunnel create connection
ngrok tcp 8022  
#To connect
ssh u0_a472@2.tcp.eu.ngrok.io -p 11049
ssh  -L 5900:localhost:5901 -N u0_a472@2.tcpeu.ngrok.io -p 11634
```

```sh
#Cloudflare
#https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm' 'cloudflared'

pkg install cloudflared

cloudflared tunnel -url localhost:8080
cloudflared tunnel --url localhost:8080 --protocol http2
```

```sh
#localxpos
5O7HgQm7LCPC9CSVjNcysefr5kx1M6D2TxxFNt4X
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
```sh
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

DocumentRoot "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs"
<Directory "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs">
data/data/com.termux/files/home/storage/Shared/A-Drive/Project

```





### php phpMyAdmin
php -S localhost:8001 -t $PREFIX/share/phpmyadmin/
```sh
pkg install php  php-apache
pkg install libsodium php-sodium #library for phpmyadmin

pkg install phpmyadmin #perfrable download version6


nano /data/data/com.termux/files/usr/share/phpmyadmin/config.inc.php

$cfg['Servers'][$i]['host'] = 'localhost:3306;
$cfg['Servers'][$i]['AllowNoPassword'] = true;
```



### mysql
> port 3306
```sh
pkg install mariadb
##setup
mariadb-install-db
mariadbd-safe -u root


mysql -u root

use mysql;
set password for 'root'@'localhost' = password('root');
flush privileges;
quit;

mysql -u root -p


Start mysql server
mysqld
background start
mysqld_safe &
mysqld_safe -u root


mysql -h localhost
```



### NodeJs mongoDb
```sh
pkg i tur-repo
pkg i mongodb
mongod #server
mongo 
pkg install nodejs
npm
```

### Java Gradle
```sh
pkg install openjdk-17
pkg install gradle
```



## Distros & Virtualization 

```sh



//Check ariticuture
uname -m      
#### Secure and Network ####



#alpine
pkg install proot-distro
proot-distro install alpine
proot-distro login alpine

#kali
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




## Hacking & Security Tools

```sh
git clone https://github.com/htr-tech/zphisher
cd zphisher
chmod +x zphisher.sh
./zphisher.sh
```





## zsh and ohmyzsh setup

```sh
pkg install zsh
chsh -s zsh  # Change shell to Zsh
termux-reload-settings
```

<details>
<summary>Setup ohmyzsh</summary>


#### To backup
```
setup_omz()  

# backup previous termux and omz files

echo -e ${RED}"[*] Setting up OMZ and termux configs..."

omz_files=(.oh-my-zsh .termux .zshrc)

for file in "${omz_files[@]}"; do
       echo -e ${CYAN}"\n[*] Backing up $file..."
        if [[ -f "$HOME/$file" || -d "$HOME/$file" ]]; then
                 { reset_color; mv -u ${HOME}/${file}{,.old}; }
        else
                 echo -e ${MAGENTA}"\n[!] $file Doesn't Exist."
        fi
done
```

#### Installing omz
```
echo -e ${CYAN}"\n[*] Installing Oh-my-zsh... \n"
{ reset_color; git clone https://github.com/robbyrussell/oh-my-zsh.git --depth 1 $HOME/.oh-my-zsh; }
cp $HOME/.oh-my-zsh/templates/zshrc.zsh-template $HOME/.zshrc
sed -i -e 's/ZSH_THEME=.*/ZSH_THEME="aditya"/g' $HOME/.zshrc
```

#### Setting up theme and configuration for omz
```
# ZSH theme
cat > $HOME/.oh-my-zsh/custom/themes/aditya.zsh-theme <<- _EOF_
# Default OMZ theme
if [[ "\$USER" == "root" ]]; then
PROMPT="%(?:%{\$fg_bold[red]%}%{\$fg_bold[yellow]%}%{\$fg_bold[red]%} :%{\$fg_bold[red]%} )"
PROMPT+='%{\$fg[cyan]%}  %c%{\$reset_color%} \$(git_prompt_info)'
else
PROMPT="%(?:%{\$fg_bold[red]%}%{\$fg_bold[green]%}%{\$fg_bold[yellow]%} :%{\$fg_bold[red]%} )"
PROMPT+='%{\$fg[cyan]%}  %c%{\$reset_color%} \$(git_prompt_info)'
fi
ZSH_THEME_GIT_PROMPT_PREFIX="%{\$fg_bold[blue]%}  git:(%{\$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{\$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{\$fg[blue]%}) %{\$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{\$fg[blue]%})"
_EOF_
# Append some aliases
cat >> $HOME/.zshrc <<- _EOF_
#------------------------------------------
alias l='ls -lh'
alias ll='ls -lah'
alias la='ls -a'
alias ld='ls -lhd'
alias p='pwd'
#alias rm='rm -rf'
alias u='cd $PREFIX'
alias h='cd $HOME'
alias :q='exit'
alias grep='grep --color=auto'
alias open='termux-open'
alias lc='lolcat'
alias xx='chmod +x'
alias rel='termux-reload-settings'
#------------------------------------------
# SSH Server Connections
# linux (Arch)
#alias arch='ssh UNAME@IP -i ~/.ssh/id_rsa.DEVICE'
# linux sftp (Arch)
#alias archfs='sftp -i ~/.ssh/id_rsa.DEVICE UNAME@IP'
_EOF_
# configuring termux
echo -e ${CYAN}"\n[*] Configuring Termux..."
if [[ ! -d "$HOME/.termux" ]]; then
mkdir $HOME/.termux
fi
# copy font
cp $(pwd)/files/.fonts/icons/dejavu-nerd-font.ttf $HOME/.termux/font.ttf
# color-scheme
cat > $HOME/.termux/colors.properties <<- _EOF_
background 		: #263238
foreground 		: #eceff1
color0  			: #263238
color8  			: #37474f
color1  			: #ff9800
color9  			: #ffa74d
color2  			: #8bc34a
color10 			: #9ccc65
color3  			: #ffc107
color11 			: #ffa000
color4  			: #03a9f4
color12 			: #81d4fa
color5  			: #e91e63
color13 			: #ad1457
color6  			: #009688
color14 			: #26a69a
color7  			: #cfd8dc
color15 			: #eceff1
_EOF_

# button config
cat > $HOME/.termux/termux.properties <<- _EOF_
extra-keys = [ \\
['ESC','|', '/', '~','HOME','UP','END'], \\
['CTRL', 'TAB', '=', '-','LEFT','DOWN','RIGHT'] \\
]
_EOF_

# change shell and reload configs
{ chsh -s zsh; } \
&& { echo -e "${GREEN}Changed shell to /bin/zsh"; } \
|| { echo -e "${MAGENTA}Failed to change shell. Please run $ chsh -s zsh"; }
{ termux-reload-settings; } \
&& { echo -e "${GREEN}Settings reloaded successfully"; } \
|| { echo -e "${MAGENTA}Failed to run $ termux-reload-settings. Restart app after installation is complete"; }
{ termux-setup-storage; } \
&& { echo -e "${GREEN}Ran termux-setup-storage successfully, you should now have a ~/storage folder"; } \
|| { echo -e "${MAGENTA}Failed to execute $ termux-setup-storage"; }

```

</details>






## pc set up

root: su -c "/system/bin/device_config set_sync_disabled_for_tests persistent; /system/bin/device_config put activity_manager max_phantom_processes 2147483647"
adb: adb shell "/system/bin/device_config set_sync_disabled_for_tests persistent; /system/bin/device_config put activity_manager max_phantom_processes 2147483647"


// Docker Termux on PC UNTESTED
// https://github.com/termux/termux-docker
docker run -it termux/termux-docker:latest