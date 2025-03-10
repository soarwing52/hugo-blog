---

title: "Asp.NET 登入機制 membership &amp; identity"

date: 2021-12-09

description: "Asp.NET 登入機制 membership &amp; identity"

---



這次要做的是在webform裡面加上登入登出以及權限機制



有分成membership跟identity兩種



membership是在2.0導入 identity是在4.X導入



直接到實作吧



https://www.youtube.com/watch?v=001xMAzNzLs&t=198s



我主要是參考這個影片，不過最大的坑，是我的VS



VS2022根本沒有加入webform的選項，後來就是用2017做開發，不愧是古蹟維護員，工具也要跟得上時代，時光倒流



資料庫的部分要先有資料表，就在C:\windows\microsoft下面有 執行之後就裝好資料表跟預存程序



會有一系列apsnet_開頭的資料表



主要就是先在webconfig裡面加上設定



    

    

        <membership defaultProvider="AspNetSqlMembershipProvider">

          <providers>

            <clear/>

            <add name="AspNetSqlMembershipProvider" type="System.Web.Security.SqlMembershipProvider"

         connectionStringName="UserConnection" enablePasswordRetrieval="false" enablePasswordReset="true" requiresQuestionAndAnswer="false" applicationName="/"

         requiresUniqueEmail="false" passwordFormat="Hashed" maxInvalidPasswordAttempts="100" minRequiredPasswordLength="6" minRequiredNonalphanumericCharacters="0"

         passwordAttemptWindow="5" passwordStrengthRegularExpression=""/>

          </providers>

        </membership>

        <profile>

          <providers>

            <clear />

            <add name="AspNetSqlProfileProvider" type="System.Web.Profile.SqlProfileProvider"

           connectionStringName="UserConnection" />

          </providers>

        </profile>

        <roleManager>

          <providers>

            <clear/>

            <add name="AspNetSqlProfileProvider" type="System.Web.Security.SqlRoleProvider"

         connectionStringName="UserConnection" />

          </providers>

        </roleManager>



然後剩下的就是用VS的tools加入 createuserwizard跟login等等



不過這裡沒有看到很直觀的怎麼改表格的layout



wizard直接就完整表格了



貫徹不用打程式就有成品?



* * *



identity我主要參考這一篇：



<https://docs.microsoft.com/en-us/aspnet/identity/overview/getting-

started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project>



基本上照著步驟做都沒問題



我是先在一個空的專案做過之後再搬到原本開發的上面，然後耍智障忘記在OWIN上面放cookie一直不登入的大禹迴圈



三過登入而不入



