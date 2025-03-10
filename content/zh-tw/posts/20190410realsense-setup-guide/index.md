---

title: "RealSense 學習筆記/教學/分享－序篇：開始與安裝"

date: 2019-04-10

description: "RealSense 學習筆記/教學/分享－序篇：開始與安裝"

---



因為工作的關係，老闆決定買了Intel D435的深度相機  

  

然後就叫我做出他們未來可以測量照片裡面物體的大小  

  

大概就是萊卡他們的產品那樣  

  

我們公司做的是道路資料蒐集road survey  

  



[youtube=https://www.youtube.com/watch?v=d6DzZXolF8Q&w=320&h=266]



  

現在能看到的主要都是F200 跟一些比較舊的教學文，新一代D400 系列的比較少  

  

所以就想來教學相長一下，希望看到這篇也能夠跟我一起交流  

\------------------------------------------------------------------------------------------------------  

求助區  

目前我卡關的有  

rs.syncer  

playback.seek  

poll_for_frames  

如果有看到的可以指導一下就拜託了!  

\-------------------------------------------------------------------------------------------------------  

  

[D400系列](https://software.intel.com/en-us/realsense/d400)  

  

我比較了D415/D435 基本上大同小異的深度相機，不過435有比較廣的視角  

  

所以在我們的需求上選用的他  

  

Rolling shutte/Global shutter是另外主要的差異  

  

不過在我們的使用上是沒有影響的  

\-------------------------------------------------------------------------------------------------------  

首先就是基礎安裝啦  

  

[開發者套件(SDK)](https://software.intel.com/en-us/realsense/sdk)  

  

這款相機主要定位是給開發者/教學/研究用途，算是把未完成的產品拿出來賣然後由使用者開發?  

不過畢竟是intel 這產品主要也是賣晶片 以供未來普及在筆電/汽車/遊戲機等  

  

  

安裝完之後 打開viewer會說需要升級韌體  

  

一樣按照連結，打開之後是個簡單的command line  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/e0170-25e6259325b725e5258f2596.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/9d9c3-25e6259325b725e5258f2596.png)



  

  

先在2確定可以升級的裝置之後會到1，輸入完整連結即可  

  

安裝完成之後裡面含有:  

Intel® RealSense™ viewer:可以直接顯示RGB/深度 2D/3D影像 並且可以錄製  

Depth quality tool:檢視深度影像  

Debug tools:裝置的校正回報時候會用到的套件  

Code samples:最一開始學習時候，就是這個Visual Studio示範開始的  

Wrappers:支援C++以外的 C、Python、Node.js API、ROS、LabVIEW語言套件  

  

打開之後開啟所有的stream 影像 可以看到紅外線IR 可見光RGB 跟深度 depth  

  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/f77c0-capture.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/55ea0-capture.png)



  

RGB相機的設定值有：  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/5006e-rgb2bcontrol.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/32d23-rgb2bcontrol.png)



有灰階：  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/0c12e-y16.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/2817d-y16.png)



RGB  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/3211c-rgb.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/b3aec-rgb.png)



BGR  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/f3919-bgr.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/f3dcb-bgr.png)



  

深度相機  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/eb715-depth2bcontrol.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/d3d3a-depth2bcontrol.png)



主要就是設定 preset還有後製post-processing  



[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/de3de-

modes.png)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/de3de-

modes.png)



hole filling是我在這次主要會用到的功能  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/31cd6-without2bhollfill.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/66946-without2bhollfill.png)



  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/dd511-hollfilled.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/9d455-hollfilled.png)



  

前後比較  

  

除了這個 還有3D模式  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/bdb03-3d.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/96b2c-3d.png)



  

一個角度當然不夠，是在做pointcloud 3D模型的時候用的  

  

然後depth viewer我用起來 就像是只用深度的部分  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/23c59-depthquality2d.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/f65e5-depthquality2d.png)



  

其他功能看起來都一樣 就是沒有RGB  

  

最後 錄製的成果可以選擇儲存在哪裡  



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/b58a1-save.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/1ac47-save.png)



  

\------------------------------------------------------------------------------------------------------------------------  

比較重要的幾個使用須知：  

一定!一定一定!!!!!! 要使用USB3.0的接孔，才有足夠的頻寬可以把相機的影像傳到電腦裡面  

  

相機本身就是一個感測器 除了鏡頭接收之外，還有小部分的硬體同步與修正，其他都回傳到電腦裡面做處理  

  

因此，在線材的選用也必須要找usb typeC 然後也是3.1的USB線才可以用，也有一些因為太長而導致了資料傳輸失敗  

最常出現的就是frame drop 當頻寬不足的時候 30fps錄下來的可能會有許多幀數遺失  

  

還有一個我看到的問題，就是快速抽差  

  

在使用的時候如果把線慢慢滑進去，可能就會偵測為USB2.0 所以務必一鼓作氣 直搗黃蓉  

  

\-----------------------------------------------------------------------------------------------------------------------  

序就寫了開始的一些步驟 還沒開始到程式碼的部分  

  

基本上安裝沒什麼問題，唯一在公司就是因為管理員權限鎖住，所以一路請主管過來輸入  

  

直到韌體升級已經花了半天，從早上十點到下午兩點了  

  

後來我問能不能調整權限，於是獲得了"新的"工作用電腦  

  

ram 4G 2014年的lenovo電腦 沒有SSD  

  

說這台完全沒有任何權限問題 你就拿去用吧!  

  



