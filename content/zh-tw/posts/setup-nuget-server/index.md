---

title: "自己架設Nuget server"

date: 2022-09-13

description: "自己架設Nuget server"

---



如果遇到通用套件，自己內部使用又不想放到外面，除了使用dll傳送檔案之外，自己架設一個nuget server就是可以使用的選項



那在看microsoft自己的官方nuget server的時候，發現是dotnet

framework，github的星星數量也才幾百，多找了幾個之後，選用Baget Server



原本的nuget server的問題就是會遇到core NET 5 6等 target framework的時候就會跑不出來



https://loic-sharma.github.io/BaGet/



在我測試Baget的時候都沒有遇到問題



不過需要再研究是 他接到database做搜尋的方式



那目前有做出來的就是



針對不同的framework



    

    

      <PropertyGroup>

        <TargetFrameworks>

    		netstandard2.1;

    		netcoreapp2.1;

    		netcoreapp2.2;

    		netcoreapp3.0;

    		netcoreapp3.1;

    		net5.0;

    		net6.0;

    	</TargetFrameworks>

        <Version>1.1.1</Version>

      </PropertyGroup>



在不同的framework用不同的dependency



    

    

    <ItemGroup Condition=" '$(TargetFramework)' == 'netcoreapp2.1' ">

        <Reference Include="System.Net" />

        <PackageReference Include="Newtonsoft.Json" Version="6.0.5" />

    </ItemGroup>

    

    <ItemGroup Condition=" '$(TargetFramework)' == 'net5.0' ">

        <Reference Include="System.Net.Http" />

        <Reference Include="System.Threading.Tasks" />

        <PackageReference Include="Newtonsoft.Json" Version="8.0.3" />

    </ItemGroup>

    

    <ItemGroup Condition=" '$(TargetFramework)' == 'net6.0' ">

        <Reference Include="System.Net.Http" />

        <Reference Include="System.Threading.Tasks" />

        <PackageReference Include="Newtonsoft.Json" Version="13.0.1" />

    </ItemGroup>



在不同framework做不同的事情



    

    

            public static string version()

            {

    #if NET5_0

                return "5.0";

    #elif NET6_0

                return "6";

    #elif NETCOREAPP2_1

                return "21";

    #else

                return "else";

    #endif

            }



