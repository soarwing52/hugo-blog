---

title: "在heroku上發布django然後用gitlab做CICD"

date: 2021-07-16

description: "在heroku上發布django然後用gitlab做CICD"

---



基本上我就是在網路上找文章 試著跟著做 做完再開始自己想怎麼改



這次找到優質好文，直接整套包好。不過這也基本上確定一件事情，用docker-

compose把DB跟service綁在一起，就是要找到能自己host一台機器比較適合



我這種免費仔就找到免費途徑就用吧



**文章連結：**<https://testdriven.io/blog/deploying-django-to-heroku-with-docker/>



我做這件事情主要是要從gitlab CICD開始，然後手上的工具是 django 或是 .net



我就還是以我比較熟悉，比較快速的django來嘗試，以前的經驗是用docker compose包著posgres不過這條路看來沒有免費方案



然後再來，就是當然用docker，比起寫heroku腳本，當然是包好docker image直接上去跑最實在



一路跟著實作，我在gitlab發布的時候



    

    

    docker login -u _ -p $HEROKU_AUTH_TOKEN registry.heroku.com



這行有出錯，但是改別的再改回來，就成功了，也不知道怎麼過的



總之，最後就是成功發布了



