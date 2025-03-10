---

title: "Raspberry Pi + Realsense: MIT App Inventor"

date: 2019-11-01

description: "Raspberry Pi + Realsense: MIT App Inventor"

---



So when I found this, this is my savior with no knowledge of JAVA I thought an

app is impossible but I found this youtube video



[youtube=https://www.youtube.com/watch?v=x10ndPdM6Vc&w=320&h=266]



this showed that with Request of Simple HTTP server and GET request I can

connect them So he's the inspiration of the whole project, thanks ADEL

KASSAH!! the app inventor is consist of two parts: GUI and code blocks This is

basically the same as Android studio, with this I can generate basic apps to

suit the need.



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/6cdf0-mit1.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/0a7ec-mit1.jpg)



In the code block session, you can see that all I did is send requests to the

server, and I set my pi to a fixed ip and a Wlan router



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/37b0d-mit2.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/908fd-mit2.jpg)



The structure of this app is really basic. The next steps are using maps I

designed my desktop app with csv which can be opened in QGIS or Google Earth

to show the roads need to take pictures and the locations of pictures taken.

one method is use app inventor to create map, while auto is on record every

point but this is not syncronized with the data in Pi, and also for an old

tablet in my company, this won't work so well. I would say the better solution

is to setup a Leaflet html templete and use also get method to show the image.

\-----------------------------------------------------------------------------------------------------------------------

So thats's quite all I used for app inventor, and what could be done. This is

an easy way out, do finish the project fast with some results. If I can

straight send the data through socket and communicate between tablet and Pi

would be great, but in current situation might not be possible.



