---

title: "Project introduction: Tablet control Raspberry Pi with Realsense Depth Camera"

date: 2019-10-30

description: "Project introduction: Tablet control Raspberry Pi with Realsense Depth Camera"

tags:
  - English posts
  - Intel Real Sense
  - Work

---

The whole Realsense D435 project started long ago, I was working with it the

whole year actually, and I have more

[records](https://soarwing52.blogspot.com/search/label/Intel%20Real%20Sense)  

  

I started on my working laptop, then need to run it on old road survey laptop,

which is a lot weaker

  

and with a long usb cable connecting from the laptop on the side seat to the

camera set on the front top of the car, it is just not as satisfying.

  

As the new pi has usb3 port, this is a good choice to update the things a bit.

  

the final result is on [Github](https://github.com/soarwing52/Remote-

Realsense)

  

### Hardware Components

  

Raspberry pi 4 4GB

Realsense D435

GPS reciever (BU353S4 in my case)

(I'm still working on how to set it up together properly)

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/a0ffa-

img_0060.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/40d06-img_0060.jpg)

  

Since the goal is to use a tablet control pi remotely without cables and a

laptop sitting on the side seat, some key are required

  

1: the connection: after asking and googling, I started with tcp sockets, and

end up with Flask REST API

  

2:Android App: at first I though I might have to program in Java for this app

for the sockets, luckily I got away with it with Flask and the [MIT App

Inventor](http://appinventor.mit.edu/) this is a website to create apps with

block code languages.

  

3.Since it can be used as HTML, how to stream the video? MJPEG is the answer I

found, for streaming IP camera was the keyword

  

The rest I can easily work with my scripts and my python skills

  

\------------------------------------------------------------------------------------------------------------------------

### The Interface

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/25fdd-

app2binterface.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/4c4d7-app2binterface.jpg)

this is made with the website MIT App inventor, the start will initialize the

whole threads

  

restart is to separate every street while surveying, because one street can be

on file

  

Auto mode will take every 15 meters one shot, instead of manually doing it

  

Drop list is to set the distance, not neccesarry to be 15 meters

  

the map opens in another page, but currently the GPS locator and how to show

more informations are too complicated, still working

\-------------------------------------------------------------------------------------------------------------------------

### The Structure

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/76daf-

flask.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/95e02-flask.jpg)

  

  

The front end will easily send GET to the Flask app, and I just define

functions, just as simple as any GUI

  

such as:

    

    

    @app.route('/auto/')  

    def auto(in_text):  

        a.command = in_text  

        return in_text

  

  

in this part I used dynamic link, so I don't need to create tons of functions  

  

I have index, video feed, commands, auto mode, photo distance five kinds

  

index is a template I tested on desktop, in the end not used

  

commands include start, restart, take a photo, and quit, falls in the decision

loop

  

start will initialize, check if gps and camera is off, and then turn it on, or

else just refresh the view

  

restart will end the camera thread and open a new one by setting mp.Value from

1 to 99 (0 is rest, 1 for running, 99 for quit)

  

photo will first send True to the checking loop, check location is not

duplicated and then send 1 to camera, after camera taken a picture will send 2

back, with log file written back to 0

  

auto mode is for the switch on the app, will send true/ false to the flask

  

drop list will send 15/25/30( still can be determined) to the camera setting

script

\-----------------------------------------------------------------------------------------------------------------------

So some things I learned in the project:

  

Flask:

before I did django, and while googling I found easy example with opencv +

flask to stream ip cameras

  

mjpeg is simply read image in bytes, while in Python every thing is an object.

  

Socket:

socket is tcp connection in the python library, though in the end I didn't use

it, but it was a try

  

when sending image will first pickle the image and the size of the image

  

while once can only send certain bytes, to avoid corrupted data, the struct

pack is needed

  

the codes can be seen [here](https://github.com/soarwing52/Remote-

Realsense/tree/master/tools%20used/socket%20image)

  

Transferring from local to flask app took me some adjustment on the controls

of commands between processes, and monitoring cpu usage for the little cpu of

pi. It still lags a bit but its the best I can get from pi

\-----------------------------------------------------------------------------------------------------------------------  

temporary result

  

[youtube=https://www.youtube.com/watch?v=8gF3X8PcEnI&w=320&h=266]

  

