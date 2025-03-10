---

title: "用GCP Compute Engine架設前後端分離（一） 前言"

date: 2023-01-16

description: "用GCP Compute Engine架設前後端分離（一） 前言"

---



從去年開始做的side project 主要是想要看看gitlab.com的CICD怎麼做的練習



同時也想要練習前端 + django rest



所以就拿我們溯溪協會的網站來練手，刻出一個新的來



那後來就順勢要變成正式的新版本上線了



介紹一下Tech stack



前端：一開始是用angular 不過後來公司開始用react，還沒刻太多頁面就直接改成React了



後端：django rest framework 主要是想用他的後台跟ORM 把DB管理這個部分簡化 如果有問題不用遠端進去下SQL就可以改資料



不過必須說 在弄migration真的是一個大問題，如果有開分枝然後都做了改動 回來很容易爆掉



中間洗掉重弄好幾次



DB:一開始就用sqllite 然後一開始在heroku上面就用heroku postgres 現在在VM上面起了一個mysql



Proxy:一開始用安裝Ubuntu Apache 後來有中文亂碼問題，改成用docker nginx-certbot



GCP: Host在Compute Engine上面 +Firewall，CICD 用Cloud Run



第一篇 還是用心得來開場好了



我覺得使用GCP VM的文章真的很少 可能是因為大家都已經覺得不用寫了 就跟一般機器差不多吧



會有資訊的都是放到cloud run上面了



Google的文件 很多 很難XD



https://cloud.google.com/build/docs/automating-builds/gitlab/build-repos-from-

gitlab



build image方面 gitlab本身可以build image 一開始也是build好之後直接傳到gitlab & heroku

repository



由於compute engine下載外部比較貴，所以比較好的做法是 把code給cloud run build起來

GCP內部下載respoitory來build image



有了image 要從VM抓 需要IAM去給權限 下docker pull 他就會說要怎麼做 照做就完成了



導流的部分，讓後端導到domain/api下面



一開始用apache 結果中文過去一直變成亂碼，就放棄用nginx



然後再加上nginx-certbot



然後要開好對外port, 接上DNS



上面每句話都花了不少時間跟精力在踩坑阿



GCP 千萬不要開最便宜的 因為真的超級不穩定...



