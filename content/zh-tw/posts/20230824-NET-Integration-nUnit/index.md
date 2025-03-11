---

title: ".NET 整合測試 nUnit"

date: 2023-08-24

description: ".NET 整合測試 nUnit"

tags:
  - dotnet
  - Work

---

這次主要的出發點是測試從原本自己刻的http client換成refit來讓程式碼更簡潔

那就必須確保資料最後都一致

可能會有的問題例如datetime的轉換

那一開始我就重新從config取設定然後注入用

    

    

    RestService.For<IGitHubApi>()

這樣的方式注入，然後用字串取得Interface

但是這樣重新注入就測不到本身設定有沒有問題，因為重新刻了

所以呢，完整的方法要用WebApplicationFactory取得application之後來用service注入測試

開一個nUnit測試專案

![](https://jaythecheyi.home.blog/wp-content/uploads/2023/08/image.png?w=1012)

然後把主API專案加到reference

![](https://jaythecheyi.home.blog/wp-content/uploads/2023/08/image-1.png?w=786)
並在API專案的CSPRJ加上

    

    

    <InternalsVisibleTo Include="$(AssemblyName).Test" />

如果是top level statement就要把Program class expose出來

    

    

    public partial class Program{}

接下來我在constructor裡面init

    

    

    var factory = new WebApplicationFactory<Program>().WithWebHostBuilder(builder => { });

    _httpClient = factory.CreateClient();

    _serviceScope = factory.Services.CreateScope();

如果要對程式本身的API做整合測試就用client打url就可以

那今天因為要測套了refit的client 所以要用serviceScope

之後的測試裡面就用

    

    

                var apiClient = _serviceScope.ServiceProvider.GetService<IAdAuthenticationApiClient>();

                var response = apiClient.Authentication(request).Result;

這樣就可以使用了

如果要用string注入type跟method就用

    

    

                Type? type = Type.GetType($"{{這裡是namespace}}, {{這裡是dll名稱}}");

                Assert.NotNull(type);

                MethodInfo? theMethod = type.GetMethod(action);

                Assert.NotNull(theMethod);

    

                var apiClient = _serviceScope.ServiceProvider.GetService(type);

                var response = await (dynamic)theMethod.Invoke(apiClient, new object[] { apiRequest });

