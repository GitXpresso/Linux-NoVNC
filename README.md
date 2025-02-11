
## running NoVNC Lubuntu-Desktop using vnc client tightvncserver
### Update and Install lubuntu-desktop and tightvncserver novnc python3-websockify python3-numpy and openssl
```bash
sudo apt-get update && sudo apt-get install -y lubuntu-desktop tightvncserver novnc python3-websockify python3-numpy openssl
```
### Create Novnc.pem using openssl
```bash
cd ~/
openssl req -x509 -nodes -newkey rsa:3072 -keyout novnc.pem -out novnc.pem -days 3650
```
### start the server
```
vncserver :1
```
run the Novnc webclient using websockify
```bash
websockify -D --web=/usr/share/novnc/ --cert=/home/ubuntu/novnc.pem 6080 localhost:5901
```
## Running Novnc LXQT using Tasksel
*Using tasksel to run lxqt in a browser since the other way wasnt working* 
look at the commit history of the readme.md to see the other way to run lxqt in the browser
### Update the System
```bash
sudo apt update
```
### install require packages
```bash
sudo apt install tasksel novnc python3-websockify python3-numpy openssl dialog -y
```
### next run tasksel, to know how to move and select in dialog heres a [tutorial](#Using-Dialog-Tutorial)
```bash
sudo tasksel
```
### Selecting LXQT looks like this
![Screenshot 2025-01-09 11.42.45 AM.png](<https://media-hosting.imagekit.io//eea7051247d9434e/Screenshot%202025-01-09%2011.42.45%20AM.png?Expires=1831049017&Key-Pair-Id=K2ZIVPTIP2VGHC&Signature=UfYNkQG1H~dBfUE9W6D89Yi-LlpgBTkD2BF6Pr6l4iUXYyid3nByOt55CC1yCxbK0HpqcW69P9Sco1wFbdvRyRyQGtS8va0G9bfRSNXAgcqgU7BPws0-ukHDvUU9r5yYXCjpZaIOSlaR0fVmhGbBTdGCaUZlgYmdlvpm8W3A39dYvFdS5Y5MZE1JQlalOMM~FkdxHpHfPvx02aRD326BEmWWAm5mIk5diW8qaBuTHHD0QnCwfkCBSxN7Z70ppOBNJeNT9cZ4-FIe6YJsEB2Nd08IsK6QwSd9s3YXBN6~UHZbcrf4D7J8yHWjE3Rq5WPdpFien2QZFlW3UhDfgt513Q__>)
### if you have any errors with tasksel go this anchor link: [Tasksel Errors][#Tasksel-Errors]
### set the Default X-Session, Type 0 as the default
```bash
sudo update-alternatives --config x-session-manager
```
### install vnc server(tigervnc)
```bash
sudo apt install tigervnc-standalone-server tigervnc-viewer -y 
```
### next setup novnc configuration using openssl
```bash
cd ~/
git clone https://github.com/Novnc/Novnc
cd Novnc
openssl req -x509 -nodes -newkey rsa:3072 -keyout novnc.pem -out novnc.pem -days 3650
```
### create the xstartup file using cat here document
```bash
cat << EOF > ~/.vnc/xstartup
#!/bin/bash
startlxqt &
EOF
```
### start a new vnc session
```bash
sudo vncserver -localhost no :1
```
### run lxqt
```bash
xtigervncviewer vncserver:5901
```
### if command upove does not work then do the command below
```bash
tigervncserver -xstartup /usr/bin/cinnamon -geometry 800x600 -localhost no :1
```
### run novnc web client using websockify, the display is :1
```bash
websockify -D --web=/usr/share/novnc/ --cert=/home/ubuntu/novnc.pem 6080 localhost:5901
```
go to `https://localhost:5901` novnc

# Running Lxqt using X11VNC Headless
## Quick Start
curl -fsSL https://bit.ly/lxqtnovnc | bash
## Tutorial
### Updating your system
```bash
sudo apt update
```
### install X11vnc Xvfb Openbox Websockify Lxqt Dialog and Novnc
```bash
sudo apt install lxqt x11vnc xvfb openbox websockify novnc dialog -y
```
### install tigervnc
```
sudo apt install tigervnc-standalone-server tigervnc-viewer -y 
```
## setup the vncserver
```
vncserver -SecurityTypes none  --I-KNOW-THIS-IS-INSECURE  -localhost no :0
```
### start the server using websockify 
```
websockify -D --web=/usr/share/novnc/ --cert=/home/ubuntu/novnc.pem 6890 localhost:5900
```
#### To have novnc automatically resize lxqt to fix your screen click the gear icon and change scaling mode to Remote Resizing

#### put 192.168.86.5 in your vnc client
#### when it says "session time out" dont worry you wont be disconnected
# What X11vnc LXQT Looks Like
![Screenshot 2025-01-09 2.15.25 PM.png](<https://media-hosting.imagekit.io//f6f47ea67be74e99/Screenshot%202025-01-09%202.15.25%20PM.png?Expires=1831058435&Key-Pair-Id=K2ZIVPTIP2VGHC&Signature=AswGv~irwiH6Myz3UXzVEZLwjC48C7~YLnxB87-D1EwLI5I08blNDFE6PFSPtYoAslsJ687c-~j4lTNGkfkpRDmPQx2om-9roqwQD4FmE8W4xjCvttKPTFz2XeMTXRNkrPA88Gz7tmWwECEmOyUZmucXHkzSR9MMOhKC2UCgrjS3KQR75INZMibV9n6QRZvktGLpRYExnIdqvNkUToxpF2CRvEUZs4bB066qqKCgRlIPqg4EOibnL3t8X8dSr0p3sJ1MRVffl6X6obXYGGLrNRUSKulZoJGHiPL5ThQfZv1CBeml21A5phbUdlciEwca28PFemMQA27jJrMQobhK6g__>)
*there is no repository that i found that runs x11vnc in a novnc web client using websockify and tigervnc*
This project was unarchived because tasksel works with the lxqt-panel works and other system settings for lxqt to work properly.
the other way to run latest version of lxqt in the browser wasnt working.
***if you dont have systemctl and other things that require dbus to work properly then it wont also work in lxqt novnc either.***
# Run LXQT using Xpra
### Install Xpra LXQT Openbox Websockify and Novnc
```bash
sudo apt install xpra lxqt openbox websockify novnc -y
```
### create the required directories
```bash
mkdir -p ~/.xdg
mkdir -p ~/.Xauthority
```
### run the xpra server
```bash
xpra start --no-daemon --bind-tcp=0.0.0.0:8989 -dbus-launch=no --start=startlxqt --socket-dir=~/.xdg
```
# What Xpra LXQT Looks like
![2025-01-09.png](<https://media-hosting.imagekit.io//fef27670cb0d41ed/2025-01-09.png?Expires=1831067284&Key-Pair-Id=K2ZIVPTIP2VGHC&Signature=vTwDCXSCmRcSLmaiGwmXrMX~~Bm1TLl5jHSBPhrVsshV82wu5-CFpvQpt53wttloaLeIv6qpoe2yahefzojFu7OkKHAPJD0lEMMSEJ14OMbo9oZcEAXpVC9~lzUKrO3q80JKxNmWllwx-icvNHrzIp2orfVoMcadM7LySqxaoSszAF4X-o2E5JXxt7QPEydPCP~vLlE094-UKdiDZJCYWSpzxSbzeLETa99FXJi1bxyETxZbmOtwjAxSxyeH2Jic8P150V03jOQemgqUUgQrDKwpaR5Gtg4~sTDjdAKP7K6FBOuyUEZdeLoSzZZKKIi9FhW0~OhqsCFtKAfcrd2CxQ__>)

# Using Dialog Tutorial
To move in dialog, use the up and down arrow keys
To Select A Option in Dialog press space
After selecting 1 or more options in Dialog Press Enter
# Tasksel Errors
## Errors with tasksel and How to fix them
### Did you get this error while installing lxqt using tasksel
```bash
Use of uninitialized value $ret[0] in string eq at /usr/bin/debconf-apt-progress line 173 <STDIN> line 4.
tasksel: apt-get failed (255)
```
### Heres how to fix the error upove by putting the follow command below in your termial
```bash
sudo apt-get install lubuntu-desktop^
```
### Did you get this error, if you did then heres how to fix it
```bash
tasksel: apt-get failed (255) 
```
### To fix the error upove, type this in your terminal
```bash
sudo dpkg --configure -a
```
### Did you get a debconf error with tasksel that looks like this heres how to fix it
```bash
debconf: DbDriver "config": /var/cache debconf/config.dat is locked by another process: Resource temporarily unavailable
```
### first put this command below in your terminal
```bash
sudo fuser -v /var/cache/debconf/config.dat
```
### output example
                     USER        PID ACCESS COMMAND

/var/cache/debconf/config.dat:
                    root      5787 F.... dpkg-preconfigu
### kill the process showed when putting the command upove in your termial
```bash
sudo kill -9 5787
```
### Answer upove does not work then do this instead
```bash
sudo rm /var/cache/debconf/*.dat 
```
### If all the answers to fix the errors dont work then install lxqt and lubuntu desktop manually
```bash
sudo apt install -y lxqt lubuntu-desktop
```
### did you get this error when running `sudo update-alternatives --config x-session-manager`
```bash
There is only one alternative in link group x-session-manager (providing /usr/bin/x-session-manager): /usr/bin/startlxqt
Nothing to configure.
```
then just create a xstartup file from [line 32](#-create-the-xstartup-file-using-cat-here-document)
## How I made this project
I made this project from the links in linksandcredits.txt file 
I copied the steps and edited them to run lxqt in the browser, part of the x11vnc I did by myself and some of it I got from the web
