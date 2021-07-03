# PHP-FPM常用配置项

### pm

* pm = static: 子进程的数量由 pm.max\_children 决定
* pm = dynamic: 
  * 子进程的数量根据以下配置动态设置 
    * pm.max\_children
    * pm.start\_servers
    * pm.min\_spare\_servers
    * pm.max\_spare\_servers
* pm = ondemand: 
  * 进程在接受请求时按需创建

### pm.max\_children

* 子进程最大数，需要根据日志以及服务器内存合理设置。当数值偏小时，请求到Nginx会无法分配到php-fpm进程导致502错误，当数值偏大时并且访问量较大时，内存被PHP吃光，降低系统响应。

### pm.start\_servers

* 动态方式下启动时的进程数
* {（空闲时最小子进程数） + （空闲时最大子进程数 - ）空闲时最小子进程数 / 2 }

### pm.min\_spare\_servers

* 动态方式下空闲进程数最小值，如果空闲进程小于此值，则创建新的子进程

### pm.max\_spare\_servers

* 动态方式下空闲进程数最大值，如果空闲进程大于此值，则进行子进程清理

### pm.max\_requests

*  **php-fpm 工作进程处理完多少请求后自动重启，主要是为了控制请求处理过程中的内存溢出** 
* 这个机制，在高并发中，经常导致 502 错误。解决方案有如下参考：
  * 值设置尽量大些，减少 PHP-CGI 重新 SPAWN 的次数，同时也能提高总体性能
  * 低峰自动重启
  * 根据日志，定时任务间隔自动重启
* 如果对当前和将来的 PHP 代码有 110％ 的信心，可以将 `pm.max_requests = 0` 与 `pm static` 一起使用

### request\_terminate\_timeout

* request\_terminate\_timeout = 60s， 0 表示没有时间限制
* 设置单个请求的超时终止时间， 0 表示没有时间限制 

### request\_slowlog\_timeout

* request\_slowlog\_timeout = 10s， 0表示不限制
* 当一个请求超时后，就会将对应的PHP调用堆栈信息完整写入到慢日志中

### slowlog

* slowlog = log/$pool.log.slow

### rlimit\_files

* 设置文件打开描述符的 rlimit 数量限制
* ulimit -n 查看

### rlimit\_core

* 设置核心rlimit最大限制值，选项: 'unlimited' 、0、正整数. 
* 默认值: 系统定义值
* 异常自启（表示60s内出现 60次 SIGSEGV orSIGBUS 异常时候，自动重启

### emergency\_restart\_threshold, emergency\_restart\_interval

* emergency\_restart\_threshold = 60
* emergency\_restart\_interval = 60s
* 表示在 emergency\_restart\_interval 所设值时间内，出现 SIGSEGV或者SIGBUS 错误的php-cgi 进程数如果超过 emergency\_restart\_threshold 个，php-fpm就会优雅重启。



### 常用命令

1. 查看php-fpm的进程个数

   ```text
   ps -ef |grep "php-fpm"|grep "pool"|wc -l
   ```

2. 查看每个php-fpm占用的内存大小

   ```text
   ps -ylC php-fpm --sort:rss
   ```

3. 查看PHP-FPM在你的机器上的平均内存占用

   ```text
   ps --no-headers -o "rss,cmd" -C php-fpm | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"M") }'
   ```

4. 查看单个php-fpm进程消耗内存的明细

   ```text
   pmap $(pgrep php-fpm) | less
   ```





