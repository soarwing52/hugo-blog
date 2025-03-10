---

title: "Docker-Compose + .Net Core + Sql Server"

date: 2021-07-15

description: "Docker-Compose + .Net Core + Sql Server"

---



這次是打算將整個 Net Core + SQL Server打包成一個docker compose 就可以直接在別台機器運行



這次我用的是我之前練習.net core + Entity Framework的範例 這樣就可以直接用migration準備好資料庫



[教學文](https://docs.microsoft.com/zh-tw/aspnet/core/data/ef-

mvc/intro?view=aspnetcore-5.0)



然後本文的github :<https://github.com/soarwing52/PortfolioJayNetCore>



然後參考了幾篇google到的docker-compose .net core sql server



前幾篇用.sh做migration的沒有成功 後來是有一篇把migration直接寫在code裡面的完成



既然是開發.net 就用Visual Studio吧



可以在開啟專案的時候就勾選支援docker 如果是既有專案，在專案按右鍵/加入/Docker支援就可以了



    

    

    #See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

    

    FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base

    WORKDIR /app

    EXPOSE 80

    EXPOSE 443

    

    FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build

    WORKDIR /src

    COPY ["PortfolioJay.csproj", "."]

    RUN dotnet restore "PortfolioJay.csproj"

    COPY . .

    WORKDIR "/src/"

    RUN dotnet build "PortfolioJay.csproj" -c Release -o /app/build

    

    FROM build AS publish

    RUN dotnet publish "PortfolioJay.csproj" -c Release -o /app/publish

    

    FROM base AS final

    WORKDIR /app

    COPY --from=publish /app/publish .

    ENTRYPOINT ["dotnet", "PortfolioJay.dll"]

    



基本上就會自動生成Dockerfile了



接下來就是docker-compose，加入/容器協調器支援



生成的docker-compose.yml



    

    

    version: '3.9'

    services:

      portfoliojay:

        image: ${DOCKER_REGISTRY-}portfoliojay

        build:

          context: .

          dockerfile: Dockerfile

    

        ports:

            - "8080:80"



然後接下來就要加入DB了，在services裡面加入以下



    

    

      db:

        image: "mcr.microsoft.com/mssql/server:2019-latest"

        environment:

           ACCEPT_EULA: "Y"

           SA_PASSWORD: "Password1@"

        ports:

            - "1433:1433"



然後在原本的container加上



    

    

        depends_on:

          - db



讓原本的容器會接到名叫db的這個容器



然後在appsettings.json裡面加入連線字串



    

    

      "ConnectionStrings": {

        "DevDB": "server=db; database=TestDB; user id=sa;password=Password1@;"

      },



接下來用entity的動作就加入context, model就在教學中



在startup.cs加入



    

    

                services.AddDbContext<SchoolContext>(options =>

    options.UseSqlServer(Configuration.GetConnectionString("DevDB")));



然後在console裡面新增migration



    

    

    add-migration AddNoteEntity

    



接下來就剩下要跑migration了



如果要更多掌控就是在開啟docker之後用bash進去



這邊就直接讓他自動化了



把原本Program.cs的static Task Main改成以下



    

    

            public async static Task Main(string[] args)

            {

                //CreateHostBuilder(args).Build().Run();

                var host = CreateHostBuilder(args).Build();

                using var scope = host.Services.CreateScope();

                var services = scope.ServiceProvider;

                try

                {

                    var dbContext = services.GetRequiredService<SchoolContext>();

                    if (dbContext.Database.IsSqlServer())

                    {

                        dbContext.Database.Migrate();

                    }

                }

                catch (Exception ex)

                {

                    var logger = scope.ServiceProvider.GetRequiredService<ILogger<Program>>();

    

                    logger.LogError(ex, "An error occurred while migrating or seeding the database.");

    

                    //throw;

                }

    

                CreateDbIfNotExists(host);

    

                // host.Run();

                await host.RunAsync();

            }



記得要加入



    

    

    using Microsoft.Extensions.DependencyInjection;

    using Microsoft.EntityFrameworkCore;



在Context這邊注意改成自己的context



以上就是簡單的把.net core跟sql server綁起來放進docker compose 以後到哪裡都可以直接跑起來 快速開始開發啦



如果需要資料 記得把bak準備好



不過我後來遇到的問題是



1.好慢，我的資料庫大概14GB 跑起來就天荒地老的久，不確定是因為我的筆電HDD太慢的關係



2.arm不支援sql

server，對，我想說用手邊的raspberry做為測試機測試發布，想做成用gitlab串接然後以後還可以遠端發布，那就只好拿我原本的django+postgres的專案來做囉



