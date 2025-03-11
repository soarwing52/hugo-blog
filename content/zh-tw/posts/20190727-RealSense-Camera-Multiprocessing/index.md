---

title: "RealSense 學習筆記/教學/分享（五）：用新的相機程式碼解釋Multiproccesing"

date: 2019-07-27

description: "RealSense 學習筆記/教學/分享（五）：用新的相機程式碼解釋Multiproccesing"

tags:
  - Intel Real Sense
  - Python
  - Work
  - 中文

---

之前完成的相機程式跟Arcmap plugin都做好之後，就正式投入使用啦

不過中間也是經過幾般波折，出去測試半天之後，還在調整

然後說想要再測試，結果直接被派了一個三天兩夜...

完全傻眼的結果，於是就在車上邊調整程式碼

然後呢，當我弄到夠自動化可以司機自己去時，我發現我沒有電腦了，因為被帶走了

於是公司又給了我一台，可是呢，果然，又是一個2014年的基本款文書機

4GB ram，HDD，唯一可以的大概就...i5 這樣

但是2014跟2018的i5，還是有差，加上HDD、ram的差別，整個電腦完全跑不動

然後，又是尚未測試，就派出去了，耶 一天的空資料

於是乎，我就開始把原本的multithread改成multiprocess

從多線程改成多進程

我就來介紹一下我的最後成果

首先，在multiprocces多個核心中，執行的模式：Pool, Proccess

Pool是作為重複執行同樣的function，然後可以指定幾個核心操作

而 Process就是分開一個child執行，可以指定 start, join, terminate

(這邊我有一個卡住的點，原來一個child 不能終止另外一個child，

我原本用一個子進程傳送指令 if xxx: break，結果直接整個freeze)

要傳輸資料的形式有幾種：

單向傳輸不限內容：

  * Queues: 這個會將資料儲存然後等候處理，用put(), get()處理 (我在用的時候，就會因為電腦處理太慢，一秒相機給30幀，但是電腦無法跟上，就會出現lag 畫面，整個慢動作，直到記憶體爆掉)

  * Pipes: 這是作為單向傳輸，指定一個輸出一個輸入，只要同時有超過一個進程在寫入或是讀取，就會錯誤

Shared memory:

  * Value: 可以輸入值，"i", "d" 分別指 integer, double兩種數字型態，我在這裡用在傳送相機/GPS的狀態，運用True/False, 不過接收端不能用 = True 更不能 is True，而是要用 =1

  * Array: 可以輸入矩陣，也是指定"i" "d" 傳輸list，我用在傳輸GPS位置 經緯度分別為double，然後照片編號，用integer

  * Manager:可以傳輸 list, dict, Namespace, Lock, RLock, Semaphore, BoundedSemaphore, Condition, Event, Queue, Value and Array.  (這個我還沒有用，不過我應該可以在未來把我的程式裡面分別的value: camera_on, camera_repeate, gps_on, gps_ready, take_a_picture 這些指令寫進一個dict來傳送)

好了 基本的元素都具備了，那就來看我怎麼處理我的程式

主要是三個 一個是

realsense，相機的操作，第二個是opencv，負責視覺化跟接收鍵盤指令，第三個則是GPS，會先輸出一個gps_on，然後就會持續送出新的經緯度資料

以下是我的傳輸圖

第一個區塊

  

Pipe |  Array |  Value  

---|---|---  

Send Image from Camera to OpenCV | Sent numerical data: Location(double, [Lontitude, Latitude]) Frame number(integer, [color, depth] | Status  

Camera_on  

GPS_on  

Camera_repeate  

  

![](https://docs.google.com/drawings/d/sEAI2h32VOm_sm_hxYxLc9g/image?w=473&h=338&rev=159&ac=1&parent=15Yw-S2pKAyLokV-AQE1tGBaelVrrLx9VSNZQoKMkZpA)
於是，我先打開child process: GPS

等到回傳GPS收到訊號，才會開啟相機

  

打開相機之後，就會再分出child, opencv

  

![](https://docs.google.com/drawings/d/solpMRrrRZbV8_Kle-Zh9RQ/image?w=585&h=475&rev=416&ac=1&parent=15Yw-S2pKAyLokV-AQE1tGBaelVrrLx9VSNZQoKMkZpA)
  

這樣設計主要是考量到，相機只要沒有最後完整的 pipeline.close()就會存檔錯誤，所以一定不能用 terminate, 放在main

process最穩

  

所以 main相機負責儲存bag,

opencv負責儲存txt,csv作為司機放進QGIS看目前位置跟已經拍過照片的位置，GPS就專門負責一直傳送新的GPS位置，不過也可以把寫目前位置的CSV丟給GPS這個做

  

然後三個process都會以 while loop持續運作，所以要避免重複下達指令的情況

是甚麼情況呢？ 就是在相機接收到第一個拍一張照片時，會花大約30毫秒完成這個動作

但是這30毫秒內，opencv會繼續下下一個拍照的指令，造成重複拍照或是如果還沒有新的GPS位置，拍出來的照片還在原位，也是不行

  

所以，我後來在opencv裡面加了一個

local_take_pic的操作，當符合條件(距離/鍵盤指令)之後，會再檢查一次是不是有更新資料，才會由value下指令拍照

另外主要還是以 if take_pic == 1 or current_location == photo_location: continue 做為保險

就是如果相機還沒回傳拍完照片，take_pic == 0(False)，或是GPS點位沒有更新，就不會繼續執行接下來的動作

  

儘管如此，這台電腦還是無法應付30幀的資料，6幀則會固定黑掉，15幀是最後的結果。

所以，向上支援比向下支援好做多了阿

  

用一台可憐的老電腦跑，真的也是

  

最後附上我的成果

  

<https://github.com/soarwing52/RealsensePython/blob/master/data_capture_qgis.py>

