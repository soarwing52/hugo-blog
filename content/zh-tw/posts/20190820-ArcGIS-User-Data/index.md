---

title: "Methods to collect data from users in ArcGIS Online: Survey123, Geoform, Crowd-source reporter app, and Smart Editor"

date: 2019-08-20

description: "Methods to collect data from users in ArcGIS Online: Survey123, Geoform, Crowd-source reporter app, and Smart Editor"

tags:
  - English posts
  - GIS
  - Work

---

  

The background of this task is to collect comments from the residents about

our planning.  

  

And the final result will be about Survey123, Geoform, Crowd-source reporter

app, and smart editor.  

  

In our system we provide a list of features as basemap and information,

combining with our analysis and planning, deployed as an ArcGIS Webmap, and

serve to the citizens.  

  

The original system before is that ObjectID is shown on the map as Labels, and

the citizens will type in their name, email, road ID and their comments. Then

the server will send this email to our company mailbox, and our staff will

collect them into one excel file.  

  

road ID on map

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/e0300-id.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/c5142-id.png)

  

  

The comment questionnaire is embed in the portal

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/67156-question.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/1addd-question.png)

  

The potential loss of data lies there, for typo in the road ID/ email address,

or server-side failure causes. And also, one person copying emails to an excel

file is simply too much manual effort.  

  

Looking into options of data collection in ArcGIS online, the first thing came

out is the Report Feature widget. (It's called Feature-Feedback in the German

version, therefore created some misunderstanding)  

  

In the [description](https://doc.arcgis.com/en/web-appbuilder/create-

apps/widget-report-feature.htm) says that it can Review Feature

(add/delete/move/reshape), and then put on notes for severity. Unfortunately

our license is not available to test this widget.  

  

  

This is NOT the function we need, this will allow users to get a more detailed

report of the features in the the map, but not giving back to the provider,

which is us. a video here show how it is:  

[youtube=https://www.youtube.com/watch?v=w031oxc21rs&w=320&h=266]

  

And later the real available functions found are the Survey123, Geoform,

Crowd-source reporter app, and Smart Editor, in this article will be a summary

and comparison. The settings will be in later articles.  

  

[Survey123](https://www.esri.com/en-us/arcgis/products/survey123/overview)

In the description of ESRI says that it is a form centric solution, working

even offline and on various platforms and languages.

  

The benefit of this is that it is really easy to create, setting-free when

using website version, can be deployed very fast.

The built-in columns for Survey123

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/b202d-survey.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/0da3f-survey.png)

  

The second great part about it is the Data panel, it will show all the data,

even showing word clouds of the replies.

  

The third benefit is the Webhook function, it can be set connecting to servers

like Integromat, Microsoft Flow, and send back replies automatically with the

desired form.

  

but the limitations of the Survey123 which eventually made us give up on it,

lies in the geopoint function.

  

As we already use sophisticated Webmaps for the citizens, we need to apply it

to our new features also, but the basemap can only be viewed without legends

and pop-ups, and the size can't be set to full screen. (Setting of the basemap

using customized Webmap will be explained in further articles)

  

And while testing, the confusion also lies in Web-mode and App-mode. The App

mode can be more customized into a JSON level. But it requires a downloading

the software Survey123 for ArcGIS(and somehow my company anti-virus says its

not trusted). Once a Suvey is edited in App-mode, it can't be edit in Web-mode

anymore. And eventhough it can accept self upload tiles from ArcMap as

basemap, it can online be shown on other apps in the Survey123 app in android

or IOS, not in web.

  

This I would say it is still more a ArcGIS-collector-like function in this

time, can't be really customized so much, but the convenience and the tidiness

is great.

  

[Geoform](https://ge-

komm.maps.arcgis.com/home/item.html?id=931653256fd24301a84fc77955914a82)

  

this is an App-template for ArcGIS Online, mostly same to the Survey123

format, but less dedicated questionnar, the format like E-mail/

Stars/Signatures are not there, and no Webhook is connected.

The good part about using Geoform is that the geopoint function can be full-

screened. And the View submittion can view the Complete Webmap information.

  

The tidiness and cross-platform/ language is still an appealing function, but

the eventual reason let us gave it up is the pop-up. In this template, pop-ups

can only set on the submitted survey points.

We have a function to show street view pictures in the pop-up. As this is the

main feature of our service, we can't give it up.

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/97a75-streetview.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/2b7ec-streetview.png)

  

I view Geoform as a more developed background for Survey123, once Survey123

added the view submit and full screen function, Geoform is out-dated.

  

[Crowd source-reporter](https://solutions.arcgis.com/local-

government/help/crowdsource-reporter/)

  

comparing to the previous two, this is a more sophisticated app-template. The

users will require a little more knowledge and ability for GIS.

As the main function of this app is 'crowdsource', therefore, collecting data

is basic.

The features worth mentioning is:

Synchronized geopoint: the point pointed on map is automatically pinned when

geopoint opened.

Like/Comment: it took a twitter-like form to create a more interactive

frontend

Pop-up edit: all the form in crowd source reporter is used in pop up,

therefore it is easy configure.

Log-in: the users can log in through ArcGIS account and other social medias

  

This template shown a professional look, and also more editable features.

  

What eventually not accepted is because it can't add the compare widget, which

the chef preffered really A LOT, and the survey form took out too much space.

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/22fb1-cs2.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/c38fd-cs2.png)

  

  

[Smart-Editor](https://doc.arcgis.com/en/web-appbuilder/create-apps/widget-

smart-editor.htm)

  

comparing to the templates, this is only a widget, so called back to basic.

  

The functions of this widget includes more setting adjustments. With the

correct adjustments, it is the final decision for our usage.

  

And the Webhook funciton is also not included, but with arcgis python API, I

set an interval of 30 mins and login, get the newly filled data, and send

formated email reply.

  

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/67573-smart.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/e8188-smart.png)

  

  

The settings will be written in later articles. In here would be a sneak peek:

Auto fill in intersect attribute, preset.

  

  

  

Conclusion

  

ESRI provides various services for users, coding and none-coding. The

complicity is to find the real way to apply the usages, though ESRI provides

detailed documentaion.

  

the comparison of usage is as this table

  

  

  

| Type| Functionality| Complicity| Customize  

---|---|---|---|---  

Survey123| Service Form(Web or App)| Webhook + Analysis| Low| Low  

Geoform| App-Template| View Submission| Low| Low  

Crowsource| App-Template| Log in | High| Mid  

Smart-Editor| Widget| More interactive attribute| Middle| High  

  

  

the App-template can be downloaded from Github and edit, then deploy on ArcGIS

Online or other server, but I'm not a Javascript programmer, so this is not

included in this article.

And also when Javascript ability is available, there is a more preferred

library called [Leaflet](https://leafletjs.com/) this provide provide the most

customization abilty, but also require high JavaScript abilty.

  

ESRI products in my opinion is offering an easier way for non-programmers to

still achieve the goal, either for spatial analysis or interactive webmap

visualization. It may not be so as imagined, but can usually create similar

output, it may not be as efficient as writing scripts, but it provides the

platform.

  

As an user of Arcmap for more than 10 years, theres benefit and also more to

improve. Learning python advanced my skill and more possibilty, but still

ArcGIS provides great service.

