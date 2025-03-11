---

title: "Creating Survey123, and the data for other ArcGIS Online usages"

date: 2019-08-20

description: "Creating Survey123, and the data for other ArcGIS Online usages"

tags:
  - English posts
  - GIS
  - Work

---

The [previous article](https://soarwing52.blogspot.com/2019/08/methods-to-

collect-data-from-users-in.html) is a summary and comparison of choices  

  

This part will explain what will be created and some usages, opinions  

  

To start with Survey123, the clearance of Web-Survey and App-Survey is

important  

  

  

  

The two I would say is completely two different service with similar looks.  

  

The web version will be created just in the browser, with various options  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/f7895-survey.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/d2b06-survey.png)

Playing in the layouts and questions, I will put on a thing I spent some time

to figure out: add my Webmap as a geopoint basemap.  

  

First create a group, share the map you want to use to this group  

  

Go to Organisation/setting/Maps set the group to your group instead of ESRI

standard  

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/455f1-setting.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/a51a0-setting.png)

  

  

And then go back to the Survey123, it will be available!

  

In the other tabs of Survey123 I would say it's really intuitive, if there'S

any question, contact me!

  

\-----------------------------------------------------------------------------------------------------------------------

  

And now the second part of this article will be about the Survey123 for ArcGIS

app

  

It is created to set in phone, tablet apps.

  

the main issue for me is the basemap, which it used difference sources. others

they are similar layout, but different base.

  

The basemap can use a hosted tile layer, or [create a tile package with

ArcMap](http://desktop.arcgis.com/en/arcmap/10.3/map/working-with-arcmap/how-

to-create-a-tile-package.htm)

  

save it in the folder of data, an then it can be uploaded, and the basemap is

shown

  

and other more detailed editting can be set in the .info file

  

[documentary](https://doc.arcgis.com/en/survey123/desktop/create-

surveys/preparebasemaps.htm)

  

with phone app it could be downloaded within the app.

\-----------------------------------------------------------------------------------------------------------------------

Once the Survey is editted in the app, the web browser can no longer edit it,

can only view the analysis and data.

  

That is the reason I say the two method is two version of Survey123, app more

similar to Collector app, while the web version is more like the google survey

form.

\-----------------------------------------------------------------------------------------------------------------------

  

now back to the content of AGOL, one folder is created with the name: Surve -

_____(name of your survey)  

  

contents are ___, ___-fieldworker, ____(type Form)  

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/dc24e-file.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/17bc4-file.png)

  

the form can be opened in the options:  

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/49629-insurvey.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/4ab68-insurvey.png)

  

And the feature layer and the fieldworker is linked in the Survey, but they

can also be added to Webmaps, and contain different data.  

  

\------------------------------------------------------------------------------------------------------------------------  

Webhook  

  

Integromat  

create one new scenrio, and log in AGOL, get the survey, then connect to the

Gmail/ Outlook or whatever trigger  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/9b84d-webhook.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/7dd39-webhook.png)

The content, reciever can use the info of the survey. here would mention one

function: ifempty  

I have a text field or number saying the road id, or description. choose one

of the two.  

  

One thing else, is Name, use the whole field instead of the First name, Last

name. Or else the space in the text field will not be accepted in integromat  

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/2b93f-ifempty.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/a2231-ifempty.png)

  

Microsoft Flow  

  

sign in, create the flow, it is prettey intuitive also.  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/55ae1-flow.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/c2eec-flow.png)

  

There are plenty more options, but for feature layer is still not available, I

created a script for usage other than Survey123 using the arcgis API.  

\------------------------------------------------------------------------------------------------------------------------  

The creation of Survey123 is quite easy, and the setting are not so

customizable.  

  

Geopoint in computer view is too small, comparing to Geoform having a full

screen function.  

  

Webhook is one of the most convenient function in AGOL, which is said to be

applied to all Feature layer in the short future.

