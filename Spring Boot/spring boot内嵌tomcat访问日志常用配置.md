# Spring Boot中tomcat access log 常用配置

```
# tomcat access log config
server:
  tomcat:
    accesslog:
      enabled: true		#是否开启日志
      directory: /tmp/accesslogs/mobile-site   #日志存储目录
      pattern: '%t %a %A %m %U%q %s %D %I %B'  #日志格式
      prefix: access		#日志文件前缀
      rename-on-rotate: true	 #是否启用日志轮转
```

## pattern的配置：

* %a - Remote IP address，远程ip地址，注意不一定是原始ip地址，中间可能经过nginx等的转发
* %A - Local IP address，本地ip
* %b - Bytes sent, excluding HTTP headers, or '-' if no bytes were sent
* %B - Bytes sent, excluding HTTP headers
* %h - Remote host name (or IP address if enableLookups for the connector is false)，远程主机名称(如果resolveHosts为false则展示IP)
* %H - Request protocol，请求协议
* %l - Remote logical username from identd (always returns '-')
* %m - Request method，请求方法（GET，POST）
* %p - Local port，接受请求的本地端口
* %q - Query string (prepended with a '?' if it exists, otherwise an empty string
* %r - First line of the request，HTTP请求的第一行（包括请求方法，请求的URI）
* %s - HTTP status code of the response，HTTP的响应代码，如：200,404
* %S - User session ID
* %t - Date and time, in Common Log Format format，日期和时间，Common Log Format格式
* %u - Remote user that was authenticated
* %U - Requested URL path
* %v - Local server name
* %D - Time taken to process the request, in millis，处理请求的时间，单位毫秒
* %T - Time taken to process the request, in seconds，处理请求的时间，单位秒
* %I - current Request thread name (can compare later with stacktraces)，当前请求的线程名，可以和打印的log对比查找问题