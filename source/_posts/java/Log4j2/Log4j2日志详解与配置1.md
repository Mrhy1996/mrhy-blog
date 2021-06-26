---
title: Log4j2日志详解与配置1
toc: true
date: 2021-05-27 10:34:11
tags: log4j2
url: log4j2-conf-1
---

Log日志在我们的开发过程中非常常见，好的日志记录能够帮助开发者快速定位问题，解决问题，现在让我们记录一下开发过程中的常用Log4j2框架日志文件配置以及参数详解

<!--more-->

# 常用的日志配置

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- log4j2配置文件与log4j(1.x版本)有很大不同 -->
<!-- status 日志级别, TRACE < DEBUG < INFO < WARN < ERROR < FATAL -->
<!-- monitorInterval 每隔300秒重新读取配置文件, 可以不重启应用的情况下修改配置 -->
<Configuration status="DEBUG" monitorInterval="300">
    <Properties>
        <property name="LOG_PATH">/app/test/logs</property>
        <Property name="LOG_LAYOUT">%-5p [%d{yyyy-MM-dd HH:mm:ss}] %t [%c:%L] - %m%n</Property>
    </Properties>

    <Appenders>
        <!-- 控制台 -->
        <Console name="console" target="SYSTEM_OUT">
            <encoder>
                <!--字符编码-->
                <charset>UTF-8</charset><!--此处设置字符集-->
            </encoder>
            <PatternLayout pattern="${LOG_LAYOUT}" />
        </Console>
        
        <RollingFile name="file_root" fileName="${LOG_PATH}/root.log" 
                        filePattern="${LOG_PATH}/$${date:yyyy-MM-dd}/root-%d{yyyy-MM-dd}-%i.log">
            <PatternLayout>
                <Pattern>${LOG_LAYOUT}</Pattern>
            </PatternLayout>
            <Policies>
                <TimeBasedTriggeringPolicy />
                <!-- 指定当文件体积大于size指定的值时, 触发Rolling -->
                <SizeBasedTriggeringPolicy size="10 MB" />
            </Policies>
            <DefaultRolloverStrategy max="20" />
        </RollingFile>
        
        <RollingFile name="file_springframework" fileName="${LOG_PATH}/springframework.log" 
                        filePattern="${LOG_PATH}/$${date:yyyy-MM-dd}/springframework-%d{yyyy-MM-dd}-%i.log">
            <PatternLayout>
                <Pattern>${LOG_LAYOUT}</Pattern>
            </PatternLayout>
            <Policies>
                <TimeBasedTriggeringPolicy />
                <SizeBasedTriggeringPolicy size="10 MB" />
            </Policies>
            <DefaultRolloverStrategy max="20" />
        </RollingFile>
        
        <RollingFile name="file_project" fileName="${LOG_PATH}/project.log" 
                        filePattern="${LOG_PATH}/$${date:yyyy-MM-dd}/project-%d{yyyy-MM-dd}-%i.log">
            <PatternLayout>
                <Pattern>${LOG_LAYOUT}</Pattern>
            </PatternLayout>
            <Policies>
                <TimeBasedTriggeringPolicy />
                <SizeBasedTriggeringPolicy size="10 MB" />
            </Policies>
            <DefaultRolloverStrategy max="20" />
        </RollingFile>
    </Appenders>

    <Loggers>
        <Logger name="com.test" level="DEBUG" additivity="false">
            <AppenderRef ref="file_project" />
            <AppenderRef ref="console" />
        </Logger>
        <Logger name="org.springframework" level="ERROR" additivity="false">
            <AppenderRef ref="file_springframework" />
            <AppenderRef ref="console" />
        </Logger>

        <Root level="ERROR">
            <AppenderRef ref="file_root" />
            <AppenderRef ref="console" />
        </Root>
    </Loggers>
</Configuration>
```

# 配置文件参数解读

1. <Configuration>

   | Attribute Name  | Description                                                  |
   | :-------------- | :----------------------------------------------------------- |
   | advertiser      | (Optional) The Advertiser plugin name which will be used to advertise individual FileAppender or SocketAppender configurations. The only Advertiser plugin provided is 'multicastdns". **广告商插件名称，将用于广告各个FileAppender或SocketAppender配置。提供的唯一广告商插件是“ multicastdns”。** |
   | dest            | Either "err" for stderr, "out" for stdout, a file path, or a URL. |
   | monitorInterval | The minimum amount of time, in seconds, that must elapse before the file configuration is checked for changes. **每隔多少秒检查配置文件** |
   | name            | The name of the configuration.**配置项名称**                 |
   | packages        | A comma separated list of package names to search for plugins. Plugins are only loaded once per classloader so changing this value may not have any effect upon reconfiguration. |
   | schema          | Identifies the location for the classloader to located the XML Schema to use to validate the configuration. Only valid when strict is set to true. If not set no schema validation will take place.**标识类加载器的位置，该位置用于定位XML模式以用于验证配置。仅在strict设置为true时有效。如果未设置，则不会进行任何模式验证。** |
   | shutdownHook    | Specifies whether or not Log4j should automatically shutdown when the JVM shuts down. The shutdown hook is enabled by default but may be disabled by setting this attribute to "disable" |
   | shutdownTimeout | Specifies how many milliseconds appenders and background tasks will get to shutdown when the JVM shuts down. Default is zero which mean that each appender uses its default timeout, and don't wait for background tasks. Not all appenders will honor this, it is a hint and not an absolute guarantee that the shutdown procedure will not take longer. Setting this too low increase the risk of losing outstanding log events not yet written to the final destination. See [LoggerContext.stop(long, java.util.concurrent.TimeUnit)](http://logging.apache.org/log4j/2.x/log4j-core/target/site/apidocs/org/apache/logging/log4j/core/LoggerContext.html#stoplong_java.util.concurrent.TimeUnit). (Not used if shutdownHook is set to "disable".) |
   | status          | The level of internal Log4j events that should be logged to the console. Valid values for this attribute are "trace", "debug", "info", "warn", "error" and "fatal". Log4j will log details about initialization, rollover and other internal actions to the status logger. Setting status="trace" is one of the first tools available to you if you need to troubleshoot log4j.(Alternatively, setting system property log4j2.debug will also print internal Log4j2 logging to the console, including internal logging that took place before the configuration file was found.) **log4j2进行初始化的，输出到控制台的日志级别** |
   | strict          | Enables the use of the strict XML format. Not supported in JSON configurations.**启用严格XML格式的使用。JSON配置中不支持。** |
   | verbose         | Enables diagnostic information while loading plugins.        |

# 同步日志和异步日志。

具体的文章请参考https://www.cnblogs.com/yeyang/p/7944906.html

总结：

| 日志输出方式   |                                                              |
| -------------- | ------------------------------------------------------------ |
| sync           | 同步打印日志，日志输出与业务逻辑在同一线程内，当日志输出完毕，才能进行后续业务逻辑操作 |
| Async Appender | 异步打印日志，内部采用ArrayBlockingQueue，对每个AsyncAppender创建一个线程用于处理日志输出。 |
| Async Logger   | 异步打印日志，采用了高性能并发框架Disruptor，创建一个线程用于处理日志输出。 |

# RollingRandomAccessFile和RollingFile区别

参考：https://blog.csdn.net/qq_33200504/article/details/78413438

启动web应用后发现输出的日志有很大一段时间不更新或者是日志文件一直是0Kb，由于需求的问题，我们需要调整日志及时输出，然后网上搜了很多大神让我把immediateFlush属性改为true，但是发现还是没有用。最后将RollingRandomAccessFile标签改成RollingFile问题解决了。然后我百度搜了RollingRandomAccessFile和RollingFile区别然后一直没有找到，最后一个外国大佬帖子上解释到（


RandomAccessFileAppender is always buffered, while FileAppender provides a config switch (bufferedIO). Both have an "immediateFlush" config option in case you want to be sure your messages are on disk (e.g. audit logging). Finally, the default buffer size for RandomAccessFileAppender is larger: 256*1024 bytes vs 8*1024 bytes for FileAppender (both appenders buffer size can be set in configuration).

）。大概意思是说log4j2的RollingRandomAccessFile 默认日志文件写入策略为异步刷盘，引出一个缓冲区（buffer）的概念，RollingRandomAccessFile 会将日志信息先写入到缓冲区，然后缓冲区满后刷到磁盘，并清空缓冲区，默认缓冲区的大小在8-256kb，具体大小需要自己设置。
