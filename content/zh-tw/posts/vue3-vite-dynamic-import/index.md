---

title: "[速寫] Vue3 + Vite的Dynamic import踩坑"

date: 2023-01-19

description: "[速寫] Vue3 + Vite的Dynamic import踩坑"

---



從一個全新的vue3專案開始 今天要放到IIS上面測



原本用一個很開心的Mapping給dynamic import的路徑 結果呢 燈燈



直接抓不到



那為甚麼呢 在打包的時候用的是rollup



他不能整個路徑都用變數 他有規範在Doc裡面 找到就解了



https://github.com/rollup/plugins/tree/master/packages/dynamic-import-

vars#limitations



基本規則：



用./開頭



要加檔案副檔名



    

    

    // Not allowed

    import(`./foo/${bar}`);

    // allowed

    import(`./foo/${bar}.js`);

    

    

    不可以純變數檔案名稱

    

    

    // not allowed

    import(`./${foo}.js`);

    // allowed

    import(`./module-${foo}.js`);



不要在變數裡面加層



    

    

    import(`./foo/${x}${y}/${z}.js`);



要直接



    

    

    import(`./foo/${x}/${y}/${z}.js`);

    

    

    另外部屬到IIS上面的時候要加web.config

    

    <?xml version="1.0" encoding="utf-8"?>

      <configuration>

          <system.webServer>

              <rewrite>

                  <rules>

    					<rule name="ignore web forms folder 1" stopProcessing="true">

    						<match url="^api/" />

    						<action type="None" />

    					</rule>

                      <rule name="Handle History Mode and custom 404/500" stopProcessing="true">

                          <match url="(.*)" />

                          <conditions logicalGrouping="MatchAll">

                              <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />

                              <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />

                          </conditions>

                          <action type="Rewrite" url="/" />

                      </rule>

                  </rules>

              </rewrite>

              <httpErrors>

                  <remove statusCode="404" subStatusCode="-1" />

                  <remove statusCode="500" subStatusCode="-1" />

                  <error statusCode="404" path="/survey/notfound" responseMode="ExecuteURL" />

                  <error statusCode="500" path="/survey/error" responseMode="ExecuteURL" />

              </httpErrors>

              <modules runAllManagedModulesForAllRequests="true"/>

          </system.webServer>

      </configuration>



