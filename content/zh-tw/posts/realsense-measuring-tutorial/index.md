---

title: "RealSense learning/turtorial/sharing blog - Chapter Five: Measuring"

date: 2019-09-16

description: "RealSense learning/turtorial/sharing blog - Chapter Five: Measuring"

---



The math of the distance between two points is really easy, just square(x^2

+y^2 +z^2)  

  

but how to implement it into the program and let it show on a GUI, then

combine with GIS platform is the task  

  



[youtube=https://www.youtube.com/watch?v=cBiSXZRpkyI&w=320&h=266]



  

  

  

So the first step is to get the x,y,z of the two ends:  

  

from x,y in the picture to x,y,z in 3D world  

  

The realsense library has pixel to point, and point to pixel, the function I

use is pixel to point  

  

rs.rs2_deproject_pixel_to_point  

  

this takes three instances, intrinsic, (x,y), and distance  

  

it's calculation is simply use the dimension from intrinsic and calculate it

into meter, therefor the input of intrinsic will be color, because we base on

the color image x,y as point  

  

the distance from camera will be defined from another function:

depth_frame.get_distance(x,y)  

  

and the output will be x,y,z  

  

def calculate_distance(self, x, y):  

color_intrin = self.color_intrin  

ix, iy = self.ix, self.iy  

udist = self.depth_frame.get_distance(ix, iy)  

vdist = self.depth_frame.get_distance(x, y)  

# print udist,vdist  

  

point1 = rs.rs2_deproject_pixel_to_point(color_intrin, [ix, iy], udist)  

point2 = rs.rs2_deproject_pixel_to_point(color_intrin, [x, y], vdist)  

# print str(point1)+str(point2)  

  

dist = math.sqrt(  

math.pow(point1[0] - point2[0], 2) + math.pow(point1[1] - point2[1], 2) +

math.pow(  

point1[2] - point2[2], 2))  

# print 'distance: '+ str(dist)  

return dist  

  

  

\----------------------------------------------------------------------------------------------------------------  

  

For GUI there was two options: matplotlib and opencv  

  

ealier this year I first start with the widget Ruler in matplotlib, and it

seems fine  

  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/0fdb2-matplotlib.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/6d717-matplotlib.png)



  

I editted this widget from simply measure pixels to real distance.  

  

at the same time, the bag file recorded by the camera contains multiple

frames, so a video mode is also possible, but with openCV.  

  

At first it was setted at Arcgis hyperlink, with different layers, this month

I updated it to a combined version, which is the video at the start.  

  

the measure in opencv is a bit different than the matplotlib:  

  

pt1, pt2 = (self.ix, self.iy), (x, y)  

ans = self.calculate_distance(x, y)  

cv2.line(img, pt1=pt1, pt2=pt2, color=(0, 0, 230), thickness=3)  

cv2.rectangle(img, rec1, rec2, (255, 255, 255), -1)  

cv2.putText(img, text, bottomLeftCornerOfText, font, fontScale, fontColor,

lineType)  

  

to show the distance  

  

I designed multie-measure record rather than just one result in the matplotlib  

  

so when we measure the width of a road, the borderline can be first drawn and

then more measurements for a more accuate result.  

  

the final result accuacy is within 10 cm  

  

The functions are:  

left click will set the start point, hold to get updated distance, when letgo

it will set the line and the distance on the screen.  

with a simple right click, the canvas is cleaned, shoing original photo  

\--------------------------------------------------------------------------------------------------------------------------  

In Arcgis the input will be  

import subprocess  

def OpenLink ( [jpg_path] ):  

bag = [jpg_path]  

  

comnd = 'python command.py -p {}'.format(bag)  

subprocess.call(comnd)  

return  

  

first call another thread to prevent crash of the main thread of GIS, prevent

data loss  

then the jpg path contains road number and frame number and the path  

  

so with simply one click the image can be shown.  

  

because matching depth takes a bit more time, and is not always needed, so I

designed to have a faster view of a road, and open the measure mode extra

while needed  

\-------------------------------------------------------------------------------------------------------------------------  

  

The current integration of Realsense and ArcGIS is almost done, good for user

I would say.  

  

I created record script, export shapefile and jpg, hyperlink measure GUI three

big parts for this camera project.  

  



