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
Usage: ab [options] [http[s]://]hostname[:port]/path
Options are:
    -n requests     Number of requests to perform
    -c concurrency  Number of multiple requests to make at a time
    -t timelimit    Seconds to max. to spend on benchmarking
                    This implies -n 50000
    -s timeout      Seconds to max. wait for each response
                    Default is 30 seconds
    -b windowsize   Size of TCP send/receive buffer, in bytes
    -B address      Address to bind to when making outgoing connections
    -p postfile     File containing data to POST. Remember also to set -T
    -u putfile      File containing data to PUT. Remember also to set -T
    -T content-type Content-type header to use for POST/PUT data, eg.
                    'application/x-www-form-urlencoded'
                    Default is 'text/plain'
    -v verbosity    How much troubleshooting info to print
    -w              Print out results in HTML tables
    -i              Use HEAD instead of GET
    -x attributes   String to insert as table attributes
    -y attributes   String to insert as tr attributes
    -z attributes   String to insert as td or th attributes
    -C attribute    Add cookie, eg. 'Apache=1234'. (repeatable)
    -H attribute    Add Arbitrary header line, eg. 'Accept-Encoding: gzip'
                    Inserted after all normal header lines. (repeatable)
    -A attribute    Add Basic WWW Authentication, the attributes
                    are a colon separated username and password.
    -P attribute    Add Basic Proxy Authentication, the attributes
                    are a colon separated username and password.
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
