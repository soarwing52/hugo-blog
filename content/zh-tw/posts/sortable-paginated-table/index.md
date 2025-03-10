---

title: "Angular Material Table 可排序、分頁的表格"

date: 2021-09-09

description: "Angular Material Table 可排序、分頁的表格"

---



好的，我的技能要新增Angular了，公司的上一個專案是單機程式，bentley Map的外掛功能，這次回到網頁的本業



雖然說是本業，但其實，也不是ESRI的地圖，也是擦屁股的其他系統，不過我覺得反而相對好，這次是Angular + Dotnet Core 2.2 +

Docker發布 + Nginx



所以我就準備來寫Angular啦



Angular + dotnet core是VS內建的選項之一 另外一個是React



Angular的優勢在於一頁式網站，並且不需要那麼大量的外接Library



然後他是一個比較嚴謹的框架，照著他的架構來使用 components, service等等



其實網路上找來找去三大框架也都各有所長，既然手上遇到的是angular就直接用



這次就是要來把原本的<table>變成可以排序的table



找到資料就是要用material table



首先依照[官網](https://material.angular.io/guide/getting-started)的說明，要先在目前的angular

project裡面加上material



    

    

    ng add @angular/material

    ng add @angular/cdk



然後才能在angular cli裡面使用



要加上一個範例的時候就用



    

    

    ng generate @angular/material:material-table --name data-table



當然name就自己取



接下來就會在app裡面產生好幾個檔案



這裡我因為不熟，第一個就是沒有ng add，我一開始沒有安裝npm -g angular，所以ng基本上都不能用



然後我切到project的ClientApp裡面之後，要用ng也不行，裝了之後又說我的local版本是8.2，global新版是12然後就用local的



這裡後來就決定更新他解決



然後又出現cant find /.angular.json 我發現有./angular.json 我後來就複製 改名成.angular.json解決



還有遇到的是More than one module matches. Use skip-import option to skip importing

the component into the closest module



解法就是加上 --module ./src/app 指定要去app就完成了



接下來就會有一個data-table資料夾 裡面就有component的css html spec.ts component.ts以及一個data-

table-datasource.ts



這些就是組成這個表格的元件，他很貼心地放好example data只要修改欄位、內容



目前我的進度就是把它先放一個我設好的json，還沒接API，改動欄位內容之後，確定可以正確顯示欄位與排序



