---

title: "Raspberry Pi - start without HDMI adapter"

date: 2019-09-25

description: "Raspberry Pi - start without HDMI adapter"

---



So, after the "mind blowing" fair we went in Stuttgart, we started to push

something new  

  

The thing is what I've been saying for a long time: Raspberry Pi 4  

  

As currently the adapter is not here yet, I tried to start without it

connecting to hdmi directly  

  

  

I did some research and found a lot of methods, wireless method hasn't succeed  

  

So, what I did on my first day of Raspberry Pi  

  



**Install Rasbian**



  

though the distributor has put a NOOB in the SD card which came with the pi, I

decided to try from step one on another SD card  

  

So go download Raspbian at <https://www.raspberrypi.org/downloads/raspbian/>  

I use the one with recommended software, for more conveniency  



[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/537bf-

raspbian.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/6182e-raspbian.png)



  

and I saw either people use Rufus or Etcher to install, I used Rufus while I

was installing Ubuntu, so I tried Etcher this time.  

  

It is quite intuitive, just get the .zip I just downloaded, and then it will

almost autmoatically find the SD card available for install, then just flash

it!  



[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/dc0fb-

etcher.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/3d9a3-etcher.png)



  

  

After flashing, the system is ready, if there is a micro-HDMI to HDMI adapter,

it can be straight connected to screen, mouse, and keyboard.  

  



**Connect to PC**



  

So two softwares are required for this step: PUTTY and Xming  

  

in the SD card first create a new file called ssh without any extentions  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/4813f-ssh.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/8d223-ssh.png)



  

the first step can test if it is working  

  

connect Pi to PC with Ethernet cable, and then use call the command line  

  

type in ipconfig /all to get all the connected port  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/95a58-networksetting.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/f03b2-networksetting.png)



  

our Pi will be at Ethernet adapter Ethernet, ip will be shown at

Autoconfiguration IPv4 Address  

  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/3a868-ipaddress.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/d7fc4-ipaddress.png)



  

  

FYI. the PCs connected to this PC will be always be in 169.254.xxx.xxx  

  

then go back and turn off Raspberry Pi, open the cmdline.txt in the SD card  

  

put in ip=169.254.xxx.xxx at the end of the line  

  

then we can put the SD card back in, turn on Pi  

  

Next step is turn on Putty, and connect with the IP address  

  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/95435-putty1.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/424e1-putty1.png)



  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/c43c3-putty2.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/3f7e0-putty2.png)



  

  

  

when the window opened, log in  

  

default is  

user: pi  

password: raspberry  

  

then the terminal is here!  



[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/a7bbf-

pi.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/e842b-pi.png)



  

for GUI, type startlxde, then we have our pi on our PC!  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/b124a-rasp.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/be390-rasp.png)



  

  

That is my first day of Raspberry Pi, next step will let it run my realsense

script!  

  

\-------------------------------------------------------------------------------------------------------------------------  



**Wireless connection**



  

On day two, I found that the ip address changes, which i will need to repeate

the process everytime I reconnect pi.  

Also connecting all the messy cables in 2019 is kind of dumb.  

  

So I looked into wireless options, and followed these two:  

Official document:

<https://www.raspberrypi.org/documentation/configuration/wireless/wireless-

cli.md>  

And a tutorial  



[youtube=https://www.youtube.com/watch?v=HbTRack54Dw&w=320&h=266]



  

  

Before connecting to wifi, the thing is that the terminal don't like

underscore(_) and space  

  

My wifi name is FRITZ!Box 7490, therefore it can't be used, so I created a

hotspot from my PC  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/c7142-hotspot.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/ee2a6-hotspot.png)



And then follow the instructions  

  

first use sudo raspi-config to connect to hot spot  

  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/8ee0d-raspi2bconfig.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/c1297-raspi2bconfig.png)



then get the sudo iwlist wlan0 scan  

  

This is to check if the connection is valid  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/ca815-scan.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/4b0b3-scan.png)



Then in the official document is kind of hard coding it  

  

with sudo nano /etc/wpa_supplicant/wpa_supplicant.conf will edit the .config

file  

  

or the automatic way used in the video is sudo wpa_passphrase "SSID"

"password"| sudo tee -a etc/wpa_supplicant/wpa_supplicant.conf



(also written in the document)



the video editted the conf file to hide the password, I skipped this step.



  



then the last step is to get the ip address



with sudo wpa_cli -i wlan0 reconfigure and then ifconfig wlan0



  



now the ip address will be shown



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/e8a3f-ifconfig.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/3ed61-ifconfig.png)



Then just open a new session on PuTTy, when the login shows up, succeed!



  



