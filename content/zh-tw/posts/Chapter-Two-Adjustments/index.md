---

title: "RealSense learning/turtorial/sharing blog - Chapter Two: More Device Adjustments"

date: 2019-04-11

description: "RealSense learning/turtorial/sharing blog - Chapter Two: More Device Adjustments"

---



So, after the hello world, more controls over the device.  



  

So the pipeline is basically start/stop, and wait_for_frames  

  

And the function of pipeline_profile I haven't know yet  

  

in this part I will put in the controls before wait_for_frames,



  



including record file, read file, and others



First, complete the configuration:  

  

Rs.config



  



  

  



enable_stream| Define the stream type and also width/height...  

---|---  

enable_all_streams| Turn on all streams at once  

enable_device| Input “serial”  

enable_device_from_file| (“filename”,True/False) for repeat_playback or not,

either once to the end, or keep looping  

enable_record_to_file| (“filename.bag”)  

disable_stream| (“stream”,”index”)  

disable_all_streams|  

  

resolve|  

  

can_resolve|  

  

  

  



the enable stream is mentions before,  

  

config.enable_stream(rs.stream.depth, 640, 360, rs.format.z16, 30)  

  

config.enable_stream(rs.stream.color, 640, 480, rs.format.bgr8, 30)  

  

The options can be found in the Intel Realsense Viewer, color,depth,infrared,

resolution, mode, fps.  

  

and the rest is more related when multiple devices were used  

config.enable_record_to_file(file_name)  

config.enable_from_device(filename)



  



these are when recording or reading



enable from device have the option True/False



allow replay or just loop once through all the frame and end



If False, it will be the Runtimeerror of "no frames arrived in 5000"



thats why I had the except in the try loop



  



\--------------------------------------------------------------------------------------------------------------------



  

device = profile.get_device()  

  

depth_sensor = device.first_depth_sensor()  

  

depth_sensor.set_option(rs.option.visual_preset, 4)  

  

dev_range = depth_sensor.get_option_range(rs.option.visual_preset)  

  

preset_name =

depth_sensor.get_option_value_description(rs.option.visual_preset, 4)  

  

<https://github.com/IntelRealSense/librealsense/wiki/D400-Series-Visual-

Presets#related-discussion>  

  

This part is setting the preset as in the Realsense Viewer, and in my need is

the preset 4, high density.  

  

The dorodnic of intel he wrote on loop to loop through the presets.  



  



which he mentioned the preset numbers are changing all the time, I suppose its

among other devices, at least in this same machine it stays  

  

recorder = device.as_recorder()  

pause = rs.recorder.pause(recorder)  

playback = device.as_playback()  

playback.set_real_time(False)  

playback.pause()  

playback.resume()  

  

Recorder will start recording when in the configuration set,with function or

.pause() and .resume()  

  

And the next is playback  

  

This is playing the recorded bag file



  



pause| Pause and resume, while resume it always turn really slow and lag,

until it catch up with the frames  

---|---  

resume  

file_name| I haven’t use these functions  

get_position  

get_duration  

is_real_time| Set to the real time behavior as recorded time  

set_real_time| Usually I set to set_real_time(False) so I can go through each

frame to measure, or else the pipeline will keep the frames looping as real

time behavior when its (True)  

config.enable_record_to_file(file_name)| So I suppose it can open one bag and

record this playback to a new file, while

rs.config.enable_device_from_file(config, '123.bag') Does Not enable the same

time config.enable_record_to_file(file_name)  

current_status|  

  

  

  



  



Intrinsic/Extrinsic



  



<https://github.com/IntelRealSense/librealsense/wiki/Projection-in-RealSense-

SDK-2.0#intrinsic-camera-parameters>



  

  

depth_stream = profile.get_stream(rs.stream.depth)  

inst = rs.video_stream_profile.intrinsics  

#get intrinsics of the frames  

depth_intrin = depth_frame.profile.as_video_stream_profile().intrinsics  

color_intrin = color_frame.profile.as_video_stream_profile().intrinsics  

depth_to_color_extrin =

depth_frame.profile.get_extrinsics_to(color_frame.profile)  

  

These are getting some data of the camera and streams, i use intrinsic to get

the calibration while projecting pixels to 3D coordinations  

  

Otherwise I have no other use for them yet, as I go through the files the

usage would be more in 3D models also, while creating by scanning, the

accuracy is a lot more important in smaller scales



\------------------------------------------------------------------------------------------------------------------



  



So this part is without example codes because it's a more general usage for

all files



  



which will be used in most further codes I will be demonstrating.



  



and I just got the use of turn off auto exposure today, will put it in in the

future



  



the usage of turning it off is for not drop frames.



  



this is also one of my major issues



  



I need to make it act as a camera, and every frame I took should be reachable



  



but the wait_for_frame is currently not giving me all the frames, and also not

recording or recorded too much



  



if any pros saw this, please write in the comments or send me an email about

this topic.



  



thank you!



  



  



  



  



