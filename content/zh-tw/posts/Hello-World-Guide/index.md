---

title: "RealSense 學習筆記/教學/分享 （一）：Hello World"

date: 2019-04-11

description: "RealSense 學習筆記/教學/分享 （一）：Hello World"

---



當然 所有程式的開始都是 Hello World 這個也不例外 第一個功能，就是開啟相機，然後偵測畫面中央的深度距離相機多遠 我的成果如下：

<https://github.com/soarwing52/RealsensePython/blob/master/phase%201/Hello%20World.py>

\----------------------------------------------------------------  求助區 目前對於

syncer poll_for_frame - 4/23已解 playback().seek - 4/24 嘗試中 這幾個我還無法成功使用

麻煩有看到的前輩指導了 然後目前還有拍照的的時候落幀的問題 必須關掉自動曝光才能夠完整錄到 但是當然需要自動曝光才能夠實際外拍啊



4/24:



auto_exposure_priority關掉就解決了

\------------------------------------------------------------------



那麼就開始吧pipeline = rs.pipeline() pipeline.start()

這就是一切的開始，pipeline就是所有影像處理的整個流程名稱 所以要開始處理影像，就是打開pipeline

![](https://lh3.googleusercontent.com/JKyhHHF3dmfM_uKOnxc4STC9ekKbS8UkVhKzpacv4BSeNJiNVv8qAoVXV7iwMgaFrd5x2kVxUKHTHb77MjHKULtH5Bfr4MxoDtfWc3BUM4ncA4BZxzg36ffisrMD0yxUI6oaGeNAY1z80a1wQw)

接下來，就是相機的參數了 config = rs.config() config.enable_stream() 開啟影像串流，讓相機傳輸影像到電腦

可以使用的參數是：(stream type,resolution, image format, fps)

config.enable_stream(rs.stream.depth, 640, 360, rs.format.z16, 30)

config.enable_stream(rs.stream.color, 640, 480, rs.format.bgr8, 30) 可以選 可見光

遠紅光 深度，基本上就是viewer 裡面看的到的選項 解析度 然後 深度就是z16 可見光可以選 RGB BGR 灰階 FPS:6/30/60

![](https://lh6.googleusercontent.com/cNA7TX0jpyibjCi3M3vLBg4Yrk1PbMa8wbxIXI79cZpPDl797WkvNjn85ket9_mdx9xIKQ25FnC5Nld7_xglN93ClJqxT4tOke6XQPvPZkS0D4zxrCd_oJDMnymxlXS4EIkN8hQvQRC2TYXgqw)

然後接下來開始擷取影像，用try加上while loop讓影像一幀接一幀送過來 Try: while True: Except Runtimeerror:

No frames came or ended Finally: pipeline.stop()



然後當沒有幀送過來時 結束這就是最基本的接收影像流程。 而接受幀數的流程 在pipeline裡面有幾個選項 frame = pipeline.wait_for_frames() depth_frame = frame.get_depth_frame() color_frame = frame.get_color_frame() 這就是我在這次使用的，先接收，然後從接收到的幀裡面取出深度以及RGB影像 而pipeline有幾個獲取幀的方式：  poll_for_frames() |  立即獲取，不過目前我還沒有能成功使用 pipeline.poll_for_frames(rs.frame()) 這是我在範例裡面看到的，不過回傳錯誤 表示我的frame裡面有兩個物件 會在frame篇裡面提到 這裡的描述是說 有畫面回傳1 沒有回傳0  

---|---  

wait_for_frames() |  他會獲取一幀之後暫停串流，然後直到獲取下一幀 不過我使用結果之後，在深度跟RGB影像配對上出了問題  

try_wait_for_frames |  這個應該就是在wait_for_frames上面再多加一個等待的秒數 沒有實測過  

  

而以上的獲取幀數方式在rs.syncer裡面也是相同的，不過目前我還沒辦法讓syncer作用 然後還有一個 rs.frame_queue()

這個是可以先把影像存在記憶體中，等到有資源時再處理，主要是避免在串流即時影像時因為資源不夠而落幀

![](https://lh3.googleusercontent.com/K1e7ORKyzfq61SrOqVe0qPe9vXwhlPMcaXrJn-IwCpH3SioRTTW4tWs_90CoH4xfb0kT335rt86qAvFo-WMojQDDrBoMAs83pNH66FZA-Oe4t31ndeFnZioUVfyGGztBfJtfF5G-2An7quL7ZA)


4/24: poll_for_frames因為可以回傳Null值，所以如果接收端有Null就會error 加上一段: if not depth_frame

or not color_frame: continue 就可以了這邊稍微提到一下，接下來是這次主要提取的一些屬性



在Hello World裡面開始有一些基本的幀的屬性 width = depth_frame.get_width() height =

depth_frame.get_height() dist = depth_frame.get_distance(width / 2, height /

2)回傳了寬度跟高度 然後找到畫面中央，然後回傳這個點的距離 接下來是frame的屬性



get_width | 寬度  

---|---  

get_height | 高度  

get_stride_in_bytes |   

get_bits_per_pixel | 每一個像素的大小  

get_bytes_per_pixel | 同上  

  

get_distance是只有rs.depth_frame才有的屬性 然後它的深度有多準呢？ 它顯示值可以到76公尺遠，實測距離最多10公尺

辦公室內我測到8公尺都還算準，但是在室外大約5~6公尺的非垂直物體 長度2公尺 誤差大約+-10公分 5公尺內大概在誤差5公分內

基本上算是可接受的誤差範圍，不過未來要計算面積的話勢必誤差會更大了 接下來就不只是pyrealsense還有opencv了 key =

cv2.waitKey(1) 這個是指每一次處理的畫面停留1豪秒 所以就是即時影像了，不論是30FPS 還是6 當值為0的時候就是停留 最後 if key

& 0xFF == ord('q') or key == 27: break 如果按Q ESC 就離開 離開之後break就會離開while loop

最後pipeline.stop() 結束整個影像串流

\-----------------------------------------------------------------------------------------------------------------------

直到第一個自己寫出的程式花了不少時間，從看懂它的資料庫跟使用方法 然後 身為一個新手 我一直到很後面 measure都寫完了 才學了class 所以在這時候

很多功能 後面要不要加() 沒有的時候是method 有的時候是object

不過直到現在，要使用沒有範例，直接從python.cpp裡面找到的功能，也還是要嘗試很多次 畢竟我沒學過C++ 所以在對照的時候也花了不少心力

才看懂了它的結構 下一步就會進入相機的設定值 在config下面可以加上很多的調控 敬請期待，也麻煩有同樣在做pyrealsense的前輩們多多指點

可以留言或是email給我囉



