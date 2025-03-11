---

title: "開始開發Bentley Mircrostation"

date: 2021-02-20

description: "開始開發Bentley Mircrostation"

tags:
  - System
  - Work

---

這個專案是基於Bentley的Microstation V8i所開發的數化系統，會著重在技術架構，因為很多細節是不能直接公開在網路上面的

會從 系統架構 然後 修改方式 設計邏輯等一一介紹

一、系統架構

從C# .net架構 設計了一個單機軟體，裡面有兩個Solution:入口、Addin

就會從入口有一個登入頁面，然後在資料庫中比對登入資料後，帳號參數給MicroStation，讓其讀取Addin並傳一個

帳號的Class裡面包含名稱、權限等等

MicroStation的讀取方法，是在C槽已經安裝好的檔案裏面，讀取Addin.dll(每次build都會把這個dll更新)，因此開發時無法使用debug

mode，都需要重新build

在C裡面的程式資料夾裡面有

config.xml放參數

ToolBar.xml是windows form的設計參數(名稱、tooltip、下給Addin.dll的指令

Taipei____.xml 是給Bentley Geospatial

Manager讀取的檔案，有Bentley的設定檔，匯出到WorkSpace資料夾讓Bentley讀取

在這邊有一個坑：因為其實拿到的xml並沒有更新，因此在使用geospatial manager save,export之後的檔案就會少圖層

因此要改就要到workspace資料夾裡面找xml改

正常流程就是都在Bentley Geospatial Manager裡面做修改

二、修改方式

在修改SQL SERVER連線的點：WorkSpace\Projects\Taipei____\xml\graphicalsources

修改圖例：WorkSpace\Projects\Taipei____\cell 裡面的.cel可以用 Bentley

Micostation的Models裡面選擇 並修改

修改完成後在criteria.xml打開 用NameCriteria修改

規則在:<https://docs.bentley.com/LiveContent/web/bentley%20map%20help-v5/en/Criteria.html>

如果不確定 可以用Bentley Geospatial Manager修改後export就可以確定語法

feature.xml裡面則是可以修改物件的attribute

* * *

這是針對在C#專案之外的修改方式，在Bentley Addin與本身的互動修改，都是一個個試出來

接下來，這個專案裡面用到的還有windows form作為toolbar設計的各種功能頁面

目前可以直接寫的筆記純粹留在這裡 Benteley真的是一個沒有多少資源的開發，未來CAD圖台的發展會往哪裡做甚麼系統

就算是用QGIS很快開發，就是會卡在GIS對於ANNOTATION放置的位置並不一定 沒有辦法達到工程圖的需求

