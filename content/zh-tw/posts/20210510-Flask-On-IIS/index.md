---

title: "IIS上跑Flask"

date: 2021-05-10

description: "IIS上跑Flask"

tags:
  - IIS
  - server
  - Work

---

公司的電腦一直跑不了apache後來就改用IIS了，因為原本的服務也都是在IIS上

現在想想，說不定在要布署的主機可以直接用，因為跟我自己的電腦一樣，限制最少

不過，最後我就是這樣設定的囉，紀錄一下

基本上就是參考這篇：

https://medium.com/ai%E5%8F%8D%E6%96%97%E5%9F%8E/iis%E9%83%A8%E7%BD%B2python-

web-%E4%BD%BF%E7%94%A8flask-89263dd8f945

[連結](https://medium.com/ai%E5%8F%8D%E6%96%97%E5%9F%8E/iis%E9%83%A8%E7%BD%B2python-

web-%E4%BD%BF%E7%94%A8flask-89263dd8f945)

主要踩到的坑，就是一開始windows server的部分功能沒有打開

好像是圖形化介面或什麼，看到錯誤訊息之後，去控制台開啟

開啟之後，怎麼試都還不行，最後直接整個ENV刪除，重新設定一個conda env

重新安裝flask opencv等等 因為一直報錯都是dll錯誤 少了numpy, opencv這幾個

還有神奇的是 用CMD打開python可以用，可是在server就不能

我還因此用了logger

https://flask.palletsprojects.com/en/1.1.x/logging/

快速抄了之後都發現是import opencv, tf這幾個有問題，還有試著在一開始不import，把import 放在一個get裡面才找到錯

大概就是這樣很簡略的，因為是二月做的，有點遺忘了

