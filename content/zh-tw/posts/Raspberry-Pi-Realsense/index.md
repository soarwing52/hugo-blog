---

title: "Raspberry Pi + Realsense: Git"

date: 2019-11-01

description: "Raspberry Pi + Realsense: Git"

---



So I finally get the chance to work on git more often. before just working one

the same laptop, or copy it to another like a noob. Since Pi is not connected

to the company network of files, and I dont have more usb sticks around, I use

git now  I learned from Corey, with this playlist



[youtube=https://www.youtube.com/watch?v=HVsySz-h9r4&w=320&h=266]



So now my current workflow would be code on Win10 with pycharm test, and then

commit, pull in Pi and then run it.



Why so complicated? because of security of my company does not allow tablet,

phones to connect to my ip address, only pi can connect to them



I can only test locally, and also pi would be the final usage, the evaluate

the performance, will be Pi



and I tried Thonny, VScode (Code-OSS) but not so smooth as I was doing on my

original laptop, so I chose to work like this.



\------------------------------------------------------------------------------------------------------------------------



1\. git init



this will start git all the status



2\. .gitignore



I would say this is really handy, I used it and I won't have to delete all the

test recorded files before commiting



like I put *.pyc, *.bag those files, or the whole folder bag, foto_log



the first two steps are setting up



Then the most of the time I just put



git add .



then git commit -m "message"



then git push



on pi



git pull



debug



then back to commit



I just got my hands on git reset hard, since I messed up some of the

configurations



this is not a full turtorial but rather a share of how I worked



Tutorials can be found already all over, just need to get the image of what

key word can do what



and also the possibility is has, and look for it when needed



