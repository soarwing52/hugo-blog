---

title: "RealSense learning/turtorial/sharing blog - Chapter Four: Frame Issues"

date: 2019-09-04

description: "RealSense learning/turtorial/sharing blog - Chapter Four: Frame Issues"

tags:
  - English posts
  - Intel Real Sense
  - Work

---

What is the next step after getting the frames?

  

while examining the collected data, there are some issues needed to be fixed,

and this post will focus on this part

  

The visualization will use openCV and the example file will be:

<https://github.com/soarwing52/RealsensePython/blob/master/phase%201/read_bag.py>

  

In the last part, we got the frames at this step

![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/7b3cb-25e5259c259625e7258925873.jpg)
  

poll_for_frames()| Will send back None value is the image is not

matchedadding: if not depth_frame or not color_frame: continuewill prevent

error while running  

---|---  

wait_for_frames()| it will automatically pair frames with order, not timestamp

or index  

So when I record the file with long gap in time, the paring is not correct  

try_wait_for_frames| it can set one time limit for wait_for_frames  

  

  

So, now is the main issue I mention for this chapter

  

the wait for frames will pair each as the color shown

  

It will match first 243 / 274, then 243 / 301, 270 / 301, 302 / 306, 302 /

333, and so on

  

So when I have one frame gap with few seconds, the content of Color and Depth

will be very different if I use wait_for_frames

  

timestamp| Frame number| Fream number| timestamp  

---|---|---|---  

402204.595| Depth243| Color 274| 402204.221  

403104.714| Depth 270| Color 301| 403104.941  

404171.521| Depth 302| Color306| 403271.741  

406038.434| Depth359| Color333| 404172.461  

407305.267| Depth 397| Color 389| 406040.621  

407338.605| Depth398| Color 427| 407308.301  

408038.697| Depth419| Color 449| 408042.221  

409238.855| Depth 455| Color 485| 409243.181  

409938.947| Depth476| Color 506| 409943.741  

410705.715| Depth 499| Color 529| 410711.021  

  

  

How did I try to fix it?

  

I recorded the frame number, and match wait_for_frame with it,

  

that means, first I wait for Depth frame number 243, if the Color frame number

is 274, it shows

  

if not, it will search until it founds it (so sometimes when the frame is

drop, it will stuck)

  

This takes a bit longer to run when the bag file is big, but at least

accurate.

  

After the first one, it will search for Depth 270 and Color 301

  

as automatically it will get 243 / 301 next, I take the 301 and then wait for

frame and get 270, skip one frame

but when it happens at 302/301, I want to get 302/306, but it will straight go

to 359 /306, I have to make the whole file run to the end, and start from the

beginning again.

  

  

This takes some time if the file is big, not so satisfying but its working for

now.

