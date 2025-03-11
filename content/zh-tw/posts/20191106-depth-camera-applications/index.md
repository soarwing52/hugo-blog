---

title: "專案簡介：深度相機的應用 從資料蒐集到資料應用 萬能的Python呀 請賜與我神奇的力量！"

date: 2019-11-06

description: "專案簡介：深度相機的應用 從資料蒐集到資料應用 萬能的Python呀 請賜與我神奇的力量！"

tags:
  - Intel Real Sense
  - Python
  - Work
  - 中文

---

這是我這半年來主要的大專案，中間用了許多不同的組成，分成許多小專案開發 目前公司的主要業務為道路調查，類似街景車的在各道路、產業道路拍照

而本案的需求是公司想要有可以測量的照片 從硬體選購-深度相機程式開發與測試-實地測試-視覺化GUI製作由我獨立完成 中間有使用到的一些程式庫有： 電腦視覺

opencv GPS Port應用 Realsense 程式庫 PyQt, Tkinter的GUI Flask app ArcGIS/QGIS 插件製作

首先是硬體的選購 由最新收到的目錄資料中找出各家廠商與比較

[PPT](https://drive.google.com/file/d/1KpiWewYsZHRE5Z2T8En6G0_wp_Hl7Rcm/view?usp=sharing)

從最高規格的 50,000歐元一台的相機 到 開發者產品的200歐元 最後選定使用Intel Realsense D435 然後我使用python開發

### Prototype

使用電腦連結相機架設於車子前方，主要功能有： 使用 QGIS來顯示目前位置 + 已拍過照的位置，底圖為衛星影像+市政府擁有的土地

自動拍照：預設距離為15公尺，可調整 手動拍照

[youtube=https://www.youtube.com/watch?v=-voe_uoZwQY&w=320&h=266]

最後PyQT的介面

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/1ac2b-pyqtcam.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/65228-pyqtcam.jpg)

[GitHub](https://github.com/soarwing52/RealsensePython)

### 平板APP

在輕便、無線的希望下，用raspberry pi4 與 Flask 做了webapp，然後使用 MIT app inventor做出平板APP使用

[youtube=https://www.youtube.com/watch?v=8gF3X8PcEnI&w=320&h=266]

由於是WebApp 所以使用 HTML 5 跟 CSS 做出簡單版的IOS介面

[youtube=https://www.youtube.com/watch?v=Hu9xVWWAcd8&w=320&h=266]

[GitHub](https://github.com/soarwing52/Remote-Realsense)

### 資料轉換

Realsense原本設計的存檔方式是 ROSbag檔案，而我加上GPS的資料記錄在CSV裡面 將BAG輸出JPEG並寫入拍攝時的位置，方便匯入GIS而做

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/5747f-processor.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/5747f-processor.jpg)

### GIS圖台銜接

用ArcGIS內的Geotagged photo to points即可出現各照片的位置 然後用Hyperlink + Python Script

然後執行程式做成的exe 即可在各介面操作測量

[youtube=https://www.youtube.com/watch?v=cBiSXZRpkyI&w=320&h=266]

[GitHub](https://github.com/soarwing52/RealsensePython)

### 影像辨識

由於每個城鎮的照片都是數萬張，所以同事花了許多時間在檢查照片中的事物 於是做了道路鋪面分類

運用Tensorflow的遷移學習，訓練了3000張四種分類的照片：路面、碎石、草地、磚地 然後輸出CSV

在ArcGIS中的SQL使用JOIN的功能即可獲得每張照片的鋪面種類

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/d3cac-

pavement.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/63b40-pavement.jpg)

[GitHub](https://github.com/soarwing52/Pavement-Classification)

### 馬賽克

最後把規畫成果與中間照的照片公開由在地民眾參與的時候，由於隱私權需要把車牌與人臉打碼 運用 ImageAI訓練好的模型與電腦視覺，找出這些區域並模糊化

[![](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/c6ba8-0717_005-1172.jpg)](https://jaythecheyi.home.blog/wp-content/uploads/2019/11/4584a-0717_005-1172.jpg)

[GitHub](https://github.com/soarwing52/Blur_for_Privacy)

* * *

這整套基本上主要都是由 Python製作，基本上可以顯現它的方便程度了

更多的詳細資料都會在本部落格的 Intel Realsense, Python等標籤中

這整套從野外資料到辦公室處理，是這半年入職對於可以改善的部分所做的應用

由於我也是新學程式剛滿一年，所以就還有很長的路要走，目前這一大專案算是告一段落

如果有任何意見與興趣歡迎跟我聯絡唷！

