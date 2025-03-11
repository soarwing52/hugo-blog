---

title: "Win10_Docker Desktop_SQL Server_Ubuntu"

date: 2021-05-10

description: "Win10_Docker Desktop_SQL Server_Ubuntu"

tags:
  - IIS
  - server
  - Work

---

這次實作的內容是 在win10環境下 建立一個Docker Container 裡面是Ubuntu的OS然後跑SQL Server

第一步：安裝好Docker Desktop

目前這個動作在Windows上已經很簡單了 相容性基本上都解決了 只要是win10版本夠新都可以

第二步：找到對應的docker image

這裡的話就是 <https://hub.docker.com/_/microsoft-mssql-server>

這是Ubuntu上面的sql server 基本上是比docker裡面跑windows跑sql server輕量許多

第三步：在container跑起一個image

docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD="SA密碼" -p 1433:1433 --name sql1

-h sql1 -d mcr.microsoft.com/mssql/server:2019-latest

輸入之後 就應該看的到跑起來 (docker ps -a)

用docker exec -it sql1 bash就可以進入確認是否有跑起來

/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "SA密碼"

然後看到 1> 就可以開始下SQL指令囉

第四步：用SSMS連接

這一關我卡最久，就是找不到正確的ip地址

我用了 0.0.0.0, localhost, docker inspect -f"{{.NetworkSettings.IPAddress}}"

sql1的地址都是錯的

最後是用ipconfig裡面 本機的 W_LAN 的 IP位置就可以了

不過沒有加port，接下來嘗試開兩個sql server container

第五步：測試多的server

需要改動的就是container 名稱 所以原本的sql1 我改成sql2 還有port 1433改成1533

在SSMS裡面的就是 ip,port的格式 不是ip:port

這樣就架設完成啦

參考文章：

https://blog.miniasp.com/post/2020/08/04/Docker-SQL-Server-on-Linux

https://mileslin.github.io/2019/04/SQL-Server-

Container-%E5%BF%AB%E9%80%9F%E5%85%A5%E9%96%80/

https://bryanyu.github.io/2018/04/11/DockerFirst/

https://docs.microsoft.com/zh-tw/sql/linux/quickstart-install-connect-

docker?view=sql-server-ver15&pivots=cs1-cmd

