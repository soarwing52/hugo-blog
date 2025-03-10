---

title: "六個禮拜做出了遙控深度相機！？ Raspberry Pi +Realsense簡介"

date: 2019-11-01

description: "六個禮拜做出了遙控深度相機！？ Raspberry Pi +Realsense簡介"

---



身為一個程式新手，有機會碰到Raspberry Pi，就想做一個越級挑戰的計畫 最終從拿到Pi到現在做出一個大致可用的Prototype 現在六個禮拜啦

現在我來大致介紹一下我的專案



[youtube=https://www.youtube.com/watch?v=8gF3X8PcEnI&w=320&h=266]



首先來介紹一下我手邊的硬體：



Intel RealSense D435：這是一個深度相機，我從大概三月到現在就是一直在處理這台，做出各項使用，主要目的是量測道路調查的結果



GPS 接收器：原本只會讓電腦接著相機做事，所以筆電需要這顆



Raspberry Pi 4 model B 4GB ram: aspberry Pi 4 model B 4GB ram:

由於深度相機需要USB3.0才有足夠的速度接收資料，所以在4代出來之後才讓我開始想做這個計畫



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/dd258-img_0060.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/38f08-img_0060.jpg)



然後技術面需要什麼呢 我原本的預想，需要能夠平板連結到Pi的無線網路，就像是一般相機wifi連結 然後如何無線傳輸影像與指令，然後就是平板的APP

我看到了這個教學，可以透過HTTP做，奠定了我整個計畫的基礎 非常感謝突尼西亞的教師[ADEL

KASSAH](https://www.youtube.com/channel/UCqWtKpyqm7oW8chdINWut8Q)



[youtube=https://www.youtube.com/watch?v=x10ndPdM6Vc&w=320&h=266]



APP Inventor上面我最後做出的結果，簡單幾個按鍵與功能 開始整個系統，每條路上Restart，FOTO手動拍照，Quit結束整段

Autouto是設定每15公尺拍一張照片，列表的部分則是可以選擇15-30公尺的頻率的選項



[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/5e6cb-

app2binterface.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/095f5-app2binterface.jpg)



所以有了App Inventor做為平板端，就要看怎麼把圖像串流上HTTP 我中間試過 Socket, SimpleHTTP兩個python library

後來找到了叫做 MJPEG這樣東西，順著找到了我最終的解方：FLASK 於是最後的架構就是 Pi上面跑Flask Server 讓平板可以用APP連接

最後加上HTML的話，就是可以通用於IOS、Android跟電腦的webapp 最後做完這個好像就是傳說中的REST API?!

然後做到現在剛好萬聖節，看到了這個網路工程師的影片，我做了個類似低配版吧 最後的網頁版APP也是看了他的影片後才想到原來可以這樣做的



[youtube=https://www.youtube.com/watch?v=B_krqlk_cXo&w=320&h=266]



然後最後我的架構大概如下



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/36868-flask.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/a60be-flask.jpg)



每個按鍵對應到的指令，然後由flask開出一個thread,然後再開出一個thread兩個process

用一個核心收GPS訊號，一個接收相機，然後用Pipe傳thread接收影像 所以總共有 指令thread, 影像thread, GPS process,

Camera process 這個架構我後來也直接應用到PYQT的程式上面，如果未來仍是覺得用電腦比較好的話，這個架構幾乎是無痛移植

有一個我這兩天才解決的，就是怎麼傳輸資訊，我用MJPG可以串流影像，但是文字需要許多的javascript 我就乾脆用 opencv直接在圖上面放文字

就解決了

\--------------------------------------------------------------------------------------------------------------------------

這大概就是我這個專案的簡介，接下來就會逐一介紹我在這中間用到的各項功能 從拿到的第一周在弄SSH，第二周處理安裝Realsense

接下來的開發反而花的時間沒那麼痛苦，倒吃甘蔗 一直覺得搞定像是 tensorflow, RS這種開發中複雜library的手續是最複雜的

然後在windows, linux (ubuntu/ debian)又都有點不同 總之，做完之後又多了幾項技能啦



