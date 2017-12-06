# ApacheBench 使用


__先了解几个关于压力测试的概念：__  
1. 吞吐率（Requests per second）  
概念：服务器并发处理能力的量化描述，单位是reqs/s，指的是某个并发用户数下单位时间内处理的请求数。某个并发用户数下单位时间内能处理的最大请求数，称之为最大吞吐率。  
计算公式：总请求数 / 处理完成这些请求数所花费的时间，即:  
```
Request per second = Complete requests / Time taken for tests
```
2. 并发连接数（The number of concurrent connections）  
概念：某个时刻服务器所接受的请求数目，简单的讲，就是一个会话。  

3. 并发用户数（The number of concurrent users，Concurrency Level）  
概念：要注意区分这个概念和并发连接数之间的区别，一个用户可能同时会产生多个会话，也即连接数。  

4. 用户平均请求等待时间（Time per request）  
计算公式：处理完成所有请求数所花费的时间/ （总请求数 / 并发用户数），即:  
Time per request = Time taken for tests /（ Complete requests / Concurrency Level）  

5. 服务器平均请求等待时间（Time per request: across all concurrent requests）  
计算公式：处理完成所有请求数所花费的时间 / 总请求数，即:  
Time taken for / testsComplete requests  
可以看到，它是吞吐率的倒数。  
同时，它也=用户平均请求等待时间/并发用户数，即:  
Time per request / Concurrency Level  

__ApacheBench 命令参数解释__  
```
Usage: ab [options] [http[s]://]hostname[:port]/path
Options are:
    -n requests     Number of requests to perform
                    //在测试会话中所执行的请求个数, 默认时仅执行一个请求。
    -c concurrency  Number of multiple requests to make at a time
                    //一次产生的请求个数（并发数）, 默认是一次一个。
    -t timelimit    Seconds to max. to spend on benchmarking
                    This implies -n 50000
                    //测试所进行的最大秒数。其内部隐含值是-n 50000。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。
    -s timeout      Seconds to max. wait for each response
                    Default is 30 seconds
    -b windowsize   Size of TCP send/receive buffer, in bytes
    -B address      Address to bind to when making outgoing connections
    -p postfile     File containing data to POST. Remember also to set -T
                    //包含了需要POST的数据的文件，文件格式如“p1=1&p2=2”.使用方法是 -p 111.txt 。 （配合-T）
    -u putfile      File containing data to PUT. Remember also to set -T
    -T content-type Content-type header to use for POST/PUT data, eg.
                    'application/x-www-form-urlencoded'
                    Default is 'text/plain'
                    //POST数据所使用的Content-type头信息，如 -T “application/x-www-form-urlencoded” 。 （配合-p）
    -v verbosity    How much troubleshooting info to print
                    //设置显示信息的详细程度 – 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。
    -w              Print out results in HTML tables
                    //以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。
    -i              Use HEAD instead of GET
                    // 执行HEAD请求，而不是GET。
    -x attributes   String to insert as table attributes
    -y attributes   String to insert as tr attributes
    -z attributes   String to insert as td or th attributes
    -C attribute    Add cookie, eg. 'Apache=1234'. (repeatable)
                    //-C cookie-name=value 对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。此参数可以重复，用逗号分割。
                    提示：可以借助session实现原理传递 JSESSIONID参数， 实现保持会话的功能，如:
                         -C ” c1=1234,c2=2,c3=3, JSESSIONID=FF056CD16DA9D71CB131C1D56F0319F8″ 。
    -H attribute    Add Arbitrary header line, eg. 'Accept-Encoding: gzip'
                    Inserted after all normal header lines. (repeatable)
    -A attribute    Add Basic WWW Authentication, the attributes
                    are a colon separated username and password.
    -P attribute    Add Basic Proxy Authentication, the attributes
                    are a colon separated username and password.
                    /-P proxy-auth-username:password 对一个中转代理提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。
    -X proxy:port   Proxyserver and port number to use
    -V              Print version number and exit
    -k              Use HTTP KeepAlive feature
    -d              Do not show percentiles served table.
    -S              Do not show confidence estimators and warnings.
    -q              Do not show progress when doing more than 150 requests
    -l              Accept variable document length (use this for dynamic pages)
    -g filename     Output collected data to gnuplot format file.
    -e filename     Output CSV file with percentages served
    -r              Don't exit on socket receive errors.
    -m method       Method name
    -h              Display usage information (this message)
    -Z ciphersuite  Specify SSL/TLS cipher suite (See openssl ciphers)
    -f protocol     Specify SSL/TLS protocol
                    (TLS1, TLS1.1, TLS1.2 or ALL)
```
