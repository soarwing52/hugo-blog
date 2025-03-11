---

title: "在Window佈署Apache &amp; Flask"

date: 2022-09-13

description: "在Window佈署Apache &amp; Flask"

tags:
  - dotnet
  - Work
  - 中文

---

這次是要佈署一個tensorflow的影像辨識在目前的系統上，既有的方式是在.NET上面呼叫bat檔案 再執行py檔完成這件事情

這樣做主要有幾個問題

速度慢：在每一次的request都需要重新開啟py檔，代表都會重新import，尤其tensorflow本身就需要時間

Debug困難：因為所有的錯都無法讀到，在.NET這邊就只會timeout error

先前的開發使用了試誤法，把環境變數 conda的環境位置都設定在bat檔案裏面才能開

    

    

    SET python_root=

    SET miniconda_root=

    SET PATH=

這些設定好之後才能順利執行

而這次由於Debug實在太麻煩，我就決定直接寫Flask API把他完成

在執行面很快就可以架好一個local host在windows server上面，但是問題是要如何正式佈署

就是在重開機等操作之後仍正常開啟

我試過了 [本篇文章](https://towardsdatascience.com/deploying-flask-on-

windows-b2839d8148fa) 是利用windows task manager自動執行bat

不過也是不好debug，於是我就看到了用Apache開Flask

於是 第一步是開啟Apache

下載好Apache (從Apache Lounge) 之後解壓縮到 C:\Apache24

執行裡面bin\httpd.exe 就會在 local host看到 it works的字樣代表成功

這裡第一個會遇到的問題就是 如果預設的80 port已經有開了什麼服務，就必須換一個port

在 conf資料夾裡面的 httpd.conf可以修改

然後接下來就是要開啟Flask

在想要的資料夾 這裡用C:\dev

我試過好像在太多層也會出一些問題

資料夾裡面就是 appname資料夾 裡面放 __init__.py; wsgi_scripts資料夾裡面放appname.wsgi

結論 windows就用IIS佈署好了

