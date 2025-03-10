---

title: "Docker Desktop 架設SQL  server"

date: 2021-05-18

description: "Docker Desktop 架設SQL  server"

---



我的環境是 win10家用版 版本20H2



安裝的DOCKER是3.33版本



使用WSL2 backend 因為之前有玩過store裡面的ubuntu所以直接沿用設定



初步測：用指令開啟



    

    

    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Password1@" -p 1433:1433 --name MSsql -d mcr.microsoft.com/mssql/server:2019-latest



就可以開啟一個名叫MSsql的Container



注意這裡如果密碼強度不符合，就會跑起container之後又exit



第二步：DockerCompose



資料夾結構



TEST_DB(下tree指令就可以囉)



    

    

    D:.

    ├─db

    │  ├─file_a.bak    

    │  │─file_b.bak     

    ├─ docker_compose.yml



我的compose如下



    

    

    version: "3.9"

       

    services:

      sqldata:

        image: mcr.microsoft.com/mssql/server:2019-latest

        container_name: ${CONTAINER_NAME}

        restart: always

        environment:

          - SA_PASSWORD=${SA_PASSWORD}

          - ACCEPT_EULA=Y

        ports:

          - "5434:1433"



然後有一個.env檔



    

    

    CONTAINER_NAME=MSsql

    SA_PASSWORD=Password1@



這裡我原本想用volumn mount來直接把bak檔案放進去，不過不知道為什麼一直失敗



於是我就放棄了，直接用複製的方式



在[stackoverflow](https://stackoverflow.com/questions/40313633/how-to-copy-

files-from-local-machine-to-docker-container-on-windows) 上指令如下



    

    

    docker cp /home/ubuntu/file_name Docker_name:/home/ 



在這邊我在win10 cmd下如下



    

    

    docker cp db\file_a.bak MSsql:/var



接下來在SSMS(SQL Server Management Studio)裡面照原來的 還原資料庫的做法就可以完成囉



其他設定：



把儲存空間移到其他位置



在我用WSL2的情況下

參考[stackoverflow](https://stackoverflow.com/questions/62441307/how-can-i-

change-the-location-of-docker-images-when-using-docker-desktop-on-

wsl2/63752264#63752264)



    

    

    wsl --export docker-desktop-data "D:\Docker\wsl\data\docker-desktop-data.tar"

    

    

    wsl --unregister docker-desktop-data

    

    

    wsl --import docker-desktop-data "D:\Docker\wsl\data" "D:\Docker\wsl\data\docker-desktop-data.tar" --version 2



剛剛存的tar可以刪除



