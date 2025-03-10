---

title: "dotnet Quartz 排程工作"

date: 2022-09-14

description: "dotnet Quartz 排程工作"

---



在找如何在後端自動排程的時候 最簡單的就是IScheduler



多找幾篇之後有幾個選項 但其實就是主要看到的文件 跟 文章都偏向quartz 就直接用了



主要參考這一篇：https://dotblogs.com.tw/wasichris/2020/12/16/172524



官方文件：https://www.quartz-scheduler.net/



基本上的架構就是 scheduler & job



開job做為要做的工作注入



自動排程



他可以設定簡單的定時/定點、日曆等等



那我選用cronexpression來讓我決那些分鐘跑一次



切換頻率



因為更改一開始注入的job不可能，因為是用cronexpression注入singelton



所以我改為兩種不同頻率的job 一個預設暫停



我在這邊有改寫scheduler 增加一個叫做 run on start



在Task StartAsync裡面linq的時候增加run on start = true



然後就分別暫停/開啟相互切換job



不過遇到的問題就是切換的時候兩個都執行了 不過目前影響不大就先這樣



