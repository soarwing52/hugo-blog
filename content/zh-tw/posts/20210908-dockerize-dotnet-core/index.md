---

title: "將dotnet core用docker包起來"

date: 2021-09-08

description: "將dotnet core用docker包起來"

tags:
  - docker
  - dotnet
  - Work

---

這次主要要做的事情是在一台遠端進去，不能連外網的機器上，發布起我們的專案，dotnetMVC+API

主要步驟就會 把專案docker化、docker compose兩支專案、然後把image,dockercompose, docker

desktop都送過去遠端電腦，然後安裝發布

其實在Visual

Studio已經有打勾就可以選docker的選項，應該很簡單就可以完成，不過並沒有，這來來回回花了差不多一個禮拜的時間把所有的問題釐清，甚至有些問題也就不知道為什麼

就放著 繞過去。

dotnet要能在docker裡面跑，必須是dotnet core以上，目前最近的是5.0，6即將發布

首先，就是在一個專案創建的時候，就可以勾選包含docker就可以產製一個包含dockerfile的專案

如果一開始沒有，在該專案按右鍵>加入

就可以加入docker支援，VS就會自動產至dockerfile，並在csproj裡面加上選項，在執行專案的時候也會出現docker的選項

在加入的地方還有一個很奇怪的選項叫做容器協調器支援，就是docker compose或是K8S的支援，按了就會自動寫出一個docker compose檔案

這些檔案在VS裡面直接按鈕執行都沒問題，但是直接用指令下docker run/docker build/docker compose都會有問題

因為VS自己會多下很多的volumn跟他會從再外面一層進去執行

所以，就來參考[docker官網](https://docs.docker.com/samples/dotnetcore/)的說明吧

我要說 目前看了[microsoft doc](https://docs.microsoft.com/zh-

tw/dotnet/core/docker/build-container?tabs=windows)我還真的是 幾乎沒有覺得他好用的時候...

所以有兩個方法，開一個dotnet容器，然後把檔案copy進去裡面，然後build

這個跟自動產製的dockerfile做法一樣，只差在路徑 ./*csproj跟 ./Project/*.csproj的差別

我在自己電腦上跑都沒問題，不過公司環境就執行的時候一直有問題 跑出EOF error就放棄了

另外一條路就是先在VS裡面publish，然後把/Release/publish的wwwroot, dll等複製進dotnet容器，entry

point設定Project.dll就可以了

這裡額外設定了port:9090

為了以後如果要一次多開幾個容器的時候 用EXPOSE 而不是用- port hostport:guestport這樣一對一的關聯

    

    

    FROM mcr.microsoft.com/dotnet/aspnet:5.0

    ENV ASPNETCORE_URLS=http://+:9090

    COPY bin/Release/net5.0/publish/ app/

    WORKDIR /app

    ENTRYPOINT ["dotnet", "UMATTUrbanAPI.dll"]

這條路就是要多做一個publish的動作然後再docker build才會有image

有了image之後，就準備要把這兩個image打包

有兩個 docker export/import 跟save/load，依照說法，export會打包container然後save是打包image

由於在VS打包的裡面，程式碼 *csproj都是用volumn從host給docker容器的，所以export的時候就沒有程式碼，沒有辦法發布

所以這邊 就 docker save image_name > name.tar

到時候把tar給遠端就可以啦

這次做完，大概就是，不要用VS做docker compose，dockerfile還可以用，不過也有部分原因是公司的電腦有問題，自己筆電上就沒問題

