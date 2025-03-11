---

title: "在win10 上跑 Flask in apache server"

date: 2021-05-10

description: "在win10 上跑 Flask in apache server"

tags:
  - IIS
  - server
  - Work

---

這篇是因為原本的.net API要做影像辨識，用到keras，而上一個做的人用C#開啟一個.bat 裡面設定跑起conda然後再執行影像辨識，得到辨識結果

開發的同事跟我說他當初摸conda環境有沒有跑起來摸很久，因為根本無法debug，得不到terminal的結果，除非他整個運算都正確

我這邊摸了一天，決定放棄，直接架一個flask跑python，效率也高不少，因為bat叫起py檔案的作法會需要每次都重新import

tf，就會很久，而且，黑盒子debug法太奇怪了

我這邊就要在windows server上面的IIS架設一個apache來host Flask

然後這個API必須只能對內，localhost外不能連到

第一步：安裝Apache

https://www.apachelounge.com/download/

下載之後解壓縮到C:\Apache

    

    

    >> cd c:\Apache24\bin

    >> httpd

看到localhost顯示It Works!就成功了

第二步：pip install mod_wsgi

會自動安裝對應的版本，裝好就好了(必須在機器自己安裝)

我這都在conda env裡面做

因為公司的測試環境有鎖住pip的連線 很多pip install都裝不了，先前都是在自己的電腦裝好之後把library或是整個conda

env放進去，不過測試過後，必須直接自己安裝才會把dll相應的都裝好

另外，我在做的時候，後來直接把env刪除重新安裝整個requirements.txt才能完整的正確安裝

接下來下這個指令

mod_wsgi-express module-config

得到的結果放進c:\Apache24\bin\httpd.conf

第三步：設定Flask

設定.py跟.wsgi

然後修正httpd.conf裡面連到的位置

還有

    

    

      # AllowOverride none  # Require all denied

參考:

https://ddoo8059.medium.com/flask-

apache-%E6%9E%B6%E5%9C%A8windows%E4%B8%8A-a47386ec913b

https://thilinamad.medium.com/flask-app-deployment-in-windows-apache-server-

mod-wsgi-82e1cfeeb2ed

