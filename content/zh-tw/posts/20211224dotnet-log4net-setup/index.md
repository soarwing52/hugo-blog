---

title: "Dotnet 加入log4net 並開始記錄"

date: 2021-12-24

description: "Dotnet 加入log4net 並開始記錄"

---



一個系統沒有LOG 就是一隻飄來飄去的金魚



有了log4net 這樣發展已久的工具就該善用



手上的系統原本有的就是用SQL自己寫了一個登入 如果登入就寫入



那 就打掉 用正式的套件吧



https://www.youtube.com/watch?v=2lAdQ_QwNww



基本上這個影片又已經說明了一切入門需要的動作



首先 nuget直接安裝log4net，用在console, web都可以



接下來就是在web.config加入設定



接下來輸入設定



這個影片說比起直接抄 祈禱會動 不如先一行一行看懂



    

    

    <log4net>

        <!-- OFF,FATAL,ERROR,WARN,INFO,DEBUG,ALL -->

        <!-- Set root logger level to ERROR and its appenders -->

        <root>

          <level value="DEBUG" />

          <appender-ref ref="SysAppender" />

        </root>

        <!-- Print only messages of level DEBUG or above in the packages -->

        <logger name="WebLogger">

          <level value="DEBUG" />

        </logger>

        <appender name="SysAppender" type="log4net.Appender.RollingFileAppender,log4net">

          <param name="File" value="log/" />

          <param name="AppendToFile" value="true" />

          <param name="RollingStyle" value="Date" />

          <param name="DatePattern" value="'demo_'yyyy_MM_dd-HH'.log'" />

          <param name="StaticLogFileName" value="false" />

          <param name="RollingStyle" value="Composite" />

          <layout type="log4net.Layout.PatternLayout,log4net">

            <param name="ConversionPattern" value="%date [th=%3thread] [line:%5L] [%-5level] %message%newlineIdentity - %identity%newlineUsername - %username%newline%location%newline%line%newline%method"/>

          </layout>

        </appender> 

      </log4net>



<level value="DEBUG" /> 是紀錄層級 DEBUG以上(基本上就是全部啦)會被記錄



<appender-ref ref="SysAppender" /> 這是加入名為sysappender的設定



所以 <appender name="SysAppender"

type="log4net.Appender.RollingFileAppender,log4net"> 設定了appender



appender 有很多種 [官網](https://logging.apache.org/log4net/release/features.html)



以WEB來說就是 放入 .net console output / .txt檔案 / DB



我這邊先寫入TXT RollingFileAppender 接下來下一段會加入DB



ConversionPattern 紀錄的值是我覺得最強大的 可以有 日期 thread 第幾行 等級 訊息這種基本的 然後再來 %identity

%username這種使用者 資訊 然後%line %method這些程式碼資訊



identity上一篇就整合好了，不知道如果是用membership會不會有



那接下來就是DB



    

    

    <appender name="AdoNetAppender" type="log4net.Appender.AdoNetAppender">

    .      <bufferSize value="1" />

          <connectionType value="System.Data.SqlClient.SqlConnection, System.Data, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />

          <connectionString value="data source=192.168.0.1; initial catalog=DB;integrated security=false;User ID=user;Password=password" />

          <commandText value="INSERT INTO Logs ([logDate],[logThread],[logLevel],[logSource],[logMessage],[exception],[user]) VALUES (@log_date, @log_thread, @log_level, @log_source, @log_message, @exception, @user)" />

          <commandType value="Text" />



除了直接用寫入外 還可以用預存程序 stored procedure



    

    

          <commandText value="dbo.procLogs_Insert" />

          <commandType value="StoredProcedure" />



這樣還能自動比對輸入欄位



設定完畢後，看看C#裡面該做什麼



    

    

    using log4net;

    using log4net.Config;

    

    private static readonly ILog log = LogManager.GetLogger(typeof(apiUserController));

    

    log.Error("this is error");



這樣就完成啦



    

    

    log4net.GlobalContext.Properties["identity"] = "testb";



然後 要設置其他變數就是用這樣設置 然後thread property會覆蓋global



設定變數就是 %property{變數名稱}



以上 報告完畢



