---

title: "Raspberry Pi + Realsense: Flask server"

date: 2019-11-01

description: "Raspberry Pi + Realsense: Flask server"

tags:
  - English posts
  - Intel Real Sense
  - Python
  - Work

---

I would start first with the easy part, Flask Flask is a micro framework to

set up at server, app and without setup like the whole Django file structure

framework. The good part is the variable they use is quite similar, because

template of jinja2 and django is very similar the code:

<https://github.com/soarwing52/Remote-Realsense/blob/master/flask_server.py>

### The Hello World

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/2c78f-html.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/b06cf-html.jpg)

    

    

    from flask import Flask, render_template, Response

    app = Flask(__name__)

    @app.route('/')

    def index():

        return "Hello World"

    

    

    if __name__ == '__main__':

    

        try:

    

            app.run('0.0.0.0')

    

        except KeyboardInterrupt:

    

            pass

    

        finally:

    

            pass

    

    

just as easy as this, a basic server is up I can connect my phone, tablet to

it

\--------------------------------------------------------------------------------------------------------------------

### The MJPEG

I was looking for how to stream image from Pi to tablet. the easiest way is

html, and how to stream, with Flask is kind of the best example I can found

def gen():

while True:

yield (b'--frame\r\n'

b'Content-Type: image/jpeg\r\n\r\n' + a.img + b'\r\n')

@app.route('/video_feed')

def video_feed():

return Response(gen(),

mimetype='multipart/x-mixed-replace; boundary=frame')

this is the example, I found with key word first found in one gist as mjpeg

because I was looking for socket possibility, and found some with SimpleHTTP

library, then the framework hit me, thanks to all the communities! so I

changed to look for stream image with flask. what I give to this need to be

image in bytes. So I used opencv

    

    

    cv2.imencode('.jpg', image)[1].tobytes()

imencode will return True/False then the image

\-----------------------------------------------------------------------------------------------------------------------

### Dynamic URL

so, to prevent writing a ton of @app.rout("/url") to set up all the GET method

I want for my commands, this comes in handy @app.route('/auto/') def

auto(in_text): a.command = in_text return in_text with this will be input to

the text when the GET method is sent. So all my commands are sent through this

method I have /cmd/ for camera control /auto/ for turn on and off the auto

camera shots /dis/ for changing the distance per photo when using auto mode

basically with Regular Expression, I can handle all other requests from

buttons, switch and spinbox because I also built html user interface for IOS

possibility This thanks to

[youtube=https://www.youtube.com/watch?v=B_krqlk_cXo&w=320&h=266]

actually I found this video when I was basically done with the whole

framework, while I use a really basic function and he is doing some fun

projects

\------------------------------------------------------------------------------------------------------------------------

### Templates

The structure is create a folder "templates" and put all the html files in it,

and it can use render_templetes function and to put in variables using {{

variable}} and

    

    

    @app.route('/')

    def index():

        return render_template(index.html, variable = "text or list")

and it can be used with if, else

    

    

    {%  if  variable =  100 %}

    

    {%  endif  %}

    

    

or dictionatry

    

    

             {% for key, value in result.iteritems() %}

    

                

    

                    {{ key }} 

    

                    {{ value }} 

    

                

    

             {% endfor %}

    

    

(<https://www.tutorialspoint.com/flask/flask_templates.htm>) I was trying to

also put the information to the html showing the current status of the system

but it cant be done with a lot of JavaScript work So I just write it in the

image with Opencv putText function

\---------------------------------------------------------------------------------------------------------------

This is basically how a server is set up, the functions I used in this

project. The main hard side is how to design the structure behind, using

threading and multiprocessing

