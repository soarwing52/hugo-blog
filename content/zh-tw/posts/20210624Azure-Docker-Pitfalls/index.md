---

title: "Azure 與 Docker的一些坑與常識"

date: 2021-06-24

description: "Azure 與 Docker的一些坑與常識"

---



這次採到的坑：



    

    

    host path ("/Users/xy/project/TEST/hi") not allowed as volume source, you need to reference an Azure File Share defined in the 'volumes' section



首先先介紹一下我做了什麼



我這次嘗試的目的是，我原本把portfolio架式在pythonanywhere上面，後來我把我的整個網站用docker

compose架了起來，所以想把docker compose看能在哪裡deploy



研究了AWS 有點複雜，就先從AZURE試試看 畢竟公司都是MS家的東西



我一開始跟了這個 [教學](https://docs.microsoft.com/zh-tw/azure/container-

instances/tutorial-docker-compose) 然後就發現 其實他是把docker

image/compose放在azure上面託管而已



應該是整體total solution的一部分



隨後，我就看了 [使用 Azure 入口網站部署具有 PostgreSQL 的 Django Web

應用程式](https://docs.microsoft.com/zh-tw/azure/developer/python/tutorial-python-

postgresql-app-portal) 然後在開啟一個postgres要一個月一千多 我就沒有繼續下去了 (原本的python

anywhere免費版也不支援postgres)



然後我就回來想繼續改我的網站，結果就發現 docker compose up跳出開頭的那個錯誤



在網路上一時也沒找到答案，後來我再重新看一次教學，剛好用了docker images這個指令



他跳出了 image is not available in this context, try use default



這個就是解藥啦!



於是找了 [Docker Context](https://docs.docker.com/engine/context/working-with-

contexts/) 的解說 用了 docker context use default 就好了



所以azure中間有默默改了context, 我把整個azure CLI都移除了也沒用



於是 這就是這次採到的坑囉



