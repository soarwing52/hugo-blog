---

title: "Roadmap: Real Sense Application"

date: 2019-11-05

description: "Roadmap: Real Sense Application"

---



The RealSense application is a solo project combined many different components

I created during my job in the road surveying company



As the idea of the project is to let colleagues can measure objects such as

road width, sidewalk, and even cracks.



Since the implementation can't simply program through it, this project has:



Hardware selection, Computer Vision, Location Service, Depth Camera Library,

GUI visualization



Further development with



ArcGIS/QGIS plugin creation, Flask web-framework



  

The first part of the selection of hardware is getting the information of the

products from the last years fair



The presentation slides are

[here](https://drive.google.com/file/d/1KpiWewYsZHRE5Z2T8En6G0_wp_Hl7Rcm/view?usp=sharing)



After the comparison of price/ development the final decision is the Intel

D435 camera



### Collection



The camera is a developer product and the prototype running video:



[youtube https://www.youtube.com/watch?v=-voe_uoZwQY&w=560&h=315]



The key features I designed:



Automation of photo shooting ( every 15 meters as default and changeable)



Live map with location and records:  

Using Google Earth/QGIS live update csv written by my script, showing the

driver where should be recorded and where is recorded



### App with Tablet



The latest development is using tablet instead of the laptop in the video

[youtube=https://www.youtube.com/watch?v=8gF3X8PcEnI&w=320&h=266] Using Flask

app and REST API to control the camera with image, gps location and camera

status showing. [GitHub](https://github.com/soarwing52/Remote-Realsense) An

Iphone Version of HTML5 + CSS



[youtube https://www.youtube.com/watch?v=Hu9xVWWAcd8&w=560&h=315]



### Data Preparation



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/5747f-processor.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/820cb-processor.jpg)



Using PyQT creating a tool for



Export JPEG, match frame records (match depth and color)



In ArcGIS import the Photos to points and it can be viewed



### GIS usage



Using hyperlink with script excute .exe, the measure plugin can be used

universal



start with fast video mode showing only color frame



measure mode will require matching frames, so takes longer while using



The bundle is on [GitHub](https://github.com/soarwing52/RealsensePython)



https://www.youtube.com/watch?v=cBiSXZRpkyI



### Image Classification



With every project over 10,000 photos, to reduce colleagues' time browsing

through all the photos



I used tensorflow and transfer learning to classify the pavements



as in the previous video, the points are classified to: pavement, tiles,

pebbles, grass four different types



Export csv file and join back in ArcGIS



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/63b40-pavement.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/d861c-pavement.jpg)



[GitHub](https://github.com/soarwing52/Pavement-Classification)



### Image Blur



One requirement for deploying the photos to public is privacy.  

Using the trained model of ImageAI, I can use computer vision and blur the

people and car license within the photos



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/c6ba8-0717_005-1172.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/4584a-0717_005-1172.jpg)



[GitHub](https://github.com/soarwing52/Blur_for_Privacy)



* * *



This part is the preparation, after deployment of data on ArcGIS Online,

Webapp design and Comment collection is also my projects further



[Link to BlogPost](https://soarwing52.blogspot.com/search/label/Survey)



