---

title: "Refit的莫名其妙的坑"

date: 2023-08-17

description: "Refit的莫名其妙的坑"

---



先順手記錄一下，卡了我兩天的坑



C# 在把API從自己刻的HttpClient換成Refit 結果好幾個API都沒有讀到BODY



一開始回傳的錯誤是:



Response status code does not indicate success: 400 (Bad Request).



然後Jerry幫忙找到



在加上LoggingHandler之後



    

    

                    var clients = services.AddRefitClient(type, settings)

                                             .AddHttpMessageHandler<LoggingHandler>()



貌似因為await request.Content.ReadAsByteArrayAsync();



把Body讀出來之後就把原本卡住的API打通了



但是馬上後面其他API也都不行



接下來就看是不是某些header 原本可能是Conent-Length



那就在Postman上面一個一個試試看



然後再輪流把該加的加進去，最後找到的罪魁禍首是



    

    

    [Headers("Accept: */*")] 



就是他 加上去之後所有API天下太平 ?嗎 接下來繼續看看



LoggingHandler  

https://github.com/reactiveui/refit/issues/258



