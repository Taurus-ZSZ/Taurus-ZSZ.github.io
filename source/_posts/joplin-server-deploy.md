---
title: joplin_server_deploy
date: 2022-05-09 08:04:18
tags:	
      -joplin
---

# joplin_server 搭建

## docker系统时间和host系统时间有drift问题解决

由于docker的时间与host主机的时间差别导致的error：

下面是我在搭建joplin_server 时遇到的问题，当时我使用的本机系统是archlinux 在docker 上部署joplin server ,当我运行容器后老是自动退出容器，我使用

```sh
docker attach joplin_server1
```

 进入容器内部他的终端输出的提示如下

```sh
[waves@waves-pc ~]$ docker attach joplin_server
Error: The device time drift is -77439ms (Max allowed: 2000ms) - cannot continue as it could cause data loss and conflicts on the sync clients. You may increase env var MAX_TIME_DRIFT to pass the check, or set to 0 to disabled the check.
    at /home/joplin/packages/server/src/app.ts:260:11
    at Generator.next (<anonymous>)
    at fulfilled (/home/joplin/packages/server/dist/app.js:5:58)
    at processTicksAndRejections (node:internal/process/task_queues:96:5)

```

### 临时解决方法：

1. 修改官方提供的.env文件，添加环境变量 MAX_TIME_DRIFT 

   ```sh
   # =============================================================================
   # PRODUCTION CONFIG EXAMPLE
   # -----------------------------------------------------------------------------
   # By default it will use SQLite, but that's mostly to test and evaluate the
   # server. So you'll want to specify db connection settings to use Postgres.
   # =============================================================================
   #
   # APP_BASE_URL=https://example.com/joplin
   # APP_PORT=22300
   # 
   # DB_CLIENT=pg
   # POSTGRES_PASSWORD=joplin
   # POSTGRES_DATABASE=joplin
   # POSTGRES_USER=joplin
   # POSTGRES_PORT=5432
   # POSTGRES_HOST=localhost
   
   # =============================================================================
   # DEV CONFIG EXAMPLE
   # -----------------------------------------------------------------------------
   # Example of local config, for development. In dev mode, you would usually use
   # SQLite so database settings are not needed.
   # =============================================================================
   #
   APP_BASE_URL=http://localhost:22300
   APP_PORT=22300
   MAX_TIME_DRIFT=1000000
   ```

2. 启动容器

   ```sh
   docker run -it --name joplin_server -v joplin:/home/joplin --env-file /home/waves/joplin/.env -p 22300:22300 joplin/server:latest
   
   ```

3. 运行结果

   ```sh
   [waves@waves-pc ~]$ docker run -it --name joplin_server -v joplin:/home/joplin --env-file /home/waves/joplin/.env -p 22300:22300 joplin/server:latest
   2022-05-09 07:56:19: App: Starting server v2.7.4 (prod) on port 22300 and PID 7...
   2022-05-09 07:56:19: App: NTP time offset: -77358ms
   2022-05-09 07:56:19: App: Running in Docker: true
   2022-05-09 07:56:19: App: Public base URL: http://localhost:22300
   2022-05-09 07:56:19: App: API base URL: http://localhost:22300
   2022-05-09 07:56:19: App: User content base URL: http://localhost:22300
   2022-05-09 07:56:19: App: Log dir: /home/joplin/packages/server/logs
   2022-05-09 07:56:19: App: DB Config: {
     client: 'sqlite3',
     name: '/home/joplin/packages/server/db-prod.sqlite',
     slowQueryLogEnabled: false,
     slowQueryLogMinDuration: 1000,
     autoMigration: true,
     asyncStackTraces: true
   }
   2022-05-09 07:56:19: App: Mailer Config: {
     enabled: false,
     host: '',
     port: 465,
     security: 'tls',
     authUser: '',
     authPassword: '********',
     noReplyName: '',
     noReplyEmail: ''
   }
   2022-05-09 07:56:19: App: Content driver: { type: 1 }
   2022-05-09 07:56:19: App: Content driver (fallback): null
   2022-05-09 07:56:19: App: Trying to connect to database...
   2022-05-09 07:56:20: App: Connection check: { latestMigration: null, isCreated: false, error: null }
   2022-05-09 07:56:27: App: Auto-migrating database...
   2022-05-09 07:56:28: App: Latest migration: { name: '20220201151223_backup_items.js', done: true }
   2022-05-09 07:56:28: App: Performing main storage check...
   2022-05-09 07:56:28: App: Database storage is special and cannot be checked this way. If the connection to the database was successful then the storage driver should work too.
   2022-05-09 07:56:28: App: Starting services...
   2022-05-09 07:56:28: ShareService: Starting maintenance...
   2022-05-09 07:56:28: EmailService: Service will be disabled because mailer config is not set or is explicitly disabled
   2022-05-09 07:56:28: TaskService: Scheduling #1 (Delete expired tokens): 0 */6 * * *
   2022-05-09 07:56:28: TaskService: Scheduling #2 (Update total sizes): 0 * * * *
   2022-05-09 07:56:28: TaskService: Scheduling #3 (Process oversized accounts): 30 */2 * * *
   2022-05-09 07:56:28: TaskService: Scheduling #7 (Compress old changes): 0 0 */2 * *
   2022-05-09 07:56:28: TaskService: Scheduling #8 (Process user deletions): 0 */6 * * *
   2022-05-09 07:56:28: App: Call this for testing: `curl http://localhost:22300/api/ping`
   2022-05-09 07:56:28: ShareService: Maintenance completed in 184ms
   2022-05-09 07:56:57: App: GET / (302) (33ms)
   2022-05-09 07:56:58: App: GET /login (200) (79ms)
   2022-05-09 07:56:58: App: GET /css/fontawesome/css/all.min.css (200) (128ms)
   2022-05-09 07:56:58: App: GET /css/bulma.min.css (200) (144ms)
   2022-05-09 07:56:58: App: GET /js/jquery.min.js (200) (141ms)
   2022-05-09 07:56:58: App: GET /css/main.css (200) (171ms)
   2022-05-09 07:56:58: App: GET /js/main.js (200) (158ms)
   2022-05-09 07:56:58: App: GET /images/Logo.png (
   ```


### 永久修改方法

​	说是永久解决，其实不是的，哈哈哈，不过比第一种方法要靠谱

1. 经过的分析我发现我的本机电脑时间跟标准网络时间不同步，有几分钟的差别。

2. 我使用的是archlinux 

   1. 查看时间的状态

      ```sh
      [waves@waves-pc ~]$ timedatectl	#检查服务状态
                     Local time: Tue 2022-05-10 23:16:45 CST
                 Universal time: Tue 2022-05-10 15:16:45 UTC
                       RTC time: Tue 2022-05-10 15:16:45
                      Time zone: Asia/Shanghai (CST, +0800)
      System clock synchronized: no	#发现没有同步
                    NTP service: inactive
                RTC in local TZ: no
      ```

   2. 同步系统时间

      ```sh
      #timedatectl set-ntp true    #将系统时间与网络时间进行同步
      [waves@waves-pc ~]$ timedatectl set-ntp true
      [waves@waves-pc ~]$ timedatectl status
                     Local time: Tue 2022-05-10 23:17:41 CST
                 Universal time: Tue 2022-05-10 15:17:41 UTC
                       RTC time: Tue 2022-05-10 15:17:41
                      Time zone: Asia/Shanghai (CST, +0800)
      System clock synchronized: yes
                    NTP service: active
                RTC in local TZ: no
      # 时间同步后发现电脑时间跟网络时间的差几分钟的现象不见了
      ```

3. 同步本地时间后运行容器的提示

   注意在运行之前，将之前的环境变量中的MAX_TIME_DRIFT=1000000注释掉奥

   ```sh
   docker run -it --name joplin_server1 -v joplin:/home/joplin --env-file /home/waves/joplin/.env -p 22300:22300 joplin/server:latest
   
   2022-05-10 15:55:07: App: Starting server v2.7.4 (prod) on port 22300 and PID 7...
   2022-05-10 15:55:07: App: NTP time offset: -13ms
   2022-05-10 15:55:07: App: Running in Docker: true
   2022-05-10 15:55:07: App: Public base URL: http://localhost:22300
   2022-05-10 15:55:07: App: API base URL: http://localhost:22300
   2022-05-10 15:55:07: App: User content base URL: http://localhost:22300
   2022-05-10 15:55:07: App: Log dir: /home/joplin/packages/server/logs
   2022-05-10 15:55:07: App: DB Config: {
     client: 'sqlite3',
     name: '/home/joplin/packages/server/db-prod.sqlite',
     slowQueryLogEnabled: false,
     slowQueryLogMinDuration: 1000,
     autoMigration: true,
     asyncStackTraces: true
   }
   2022-05-10 15:55:07: App: Mailer Config: {
     enabled: false,
     host: '',
     port: 465,
     security: 'tls',
     authUser: '',
     authPassword: '********',
     noReplyName: '',
     noReplyEmail: ''
   }
   2022-05-10 15:55:07: App: Content driver: { type: 1 }
   
   ```

##  默认邮箱与密码

```sh
admin@localhost
admin
```



## Docker 部署joplin server 

###  joplin_server Docker 部署流程

[官方文档](https://github.com/laurent22/joplin/tree/dev/packages/server)

**前提条件：**

1. 安装服务器上安装 docker
2. 准备域名（可选）

**安装部署**

1. 下载最新的joplin server docker images 

   - joplin server docker [链接](https://hub.docker.com/r/joplin/server) 

     ```sh
     docker pull joplin/server
     
     ```

2. joplin docker 环境变量文件

   1. `.env 文件`

      ```sh
      # =============================================================================
      # PRODUCTION CONFIG EXAMPLE
      # -----------------------------------------------------------------------------
      # By default it will use SQLite, but that's mostly to test and evaluate the
      # server. So you'll want to specify db connection settings to use Postgres.
      # =============================================================================
      #
      # APP_BASE_URL=https://example.com/joplin
      # APP_PORT=22300
      # 
      # DB_CLIENT=pg
      # POSTGRES_PASSWORD=joplin
      # POSTGRES_DATABASE=joplin
      # POSTGRES_USER=joplin
      # POSTGRES_PORT=5432
      # POSTGRES_HOST=localhost
      
      # =============================================================================
      # DEV CONFIG EXAMPLE
      # -----------------------------------------------------------------------------
      # Example of local config, for development. In dev mode, you would usually use
      # SQLite so database settings are not needed.
      # =============================================================================
      #
      APP_BASE_URL=http://localhost:22301	#备注这个域名或者IP地址一定要是被访问的那个而且这个端口是访问者的端口号， 使用http://localhost:22301 访问
      APP_PORT=22300	#这个是docker server 的
      # MAX_TIME_DRIFT=1000000
      ```

3. 运行容器

   ```sh
   docker run -d --name joplin_server -v joplin:/home/joplin --env-file /home/proton/joplin/.env -p 22300:22300 joplin/server:latest
   
   docker run -it --name joplin_server -v joplin:/home/joplin --env-file /home/waves/joplin/.env -p 22300:22300 joplin/server:latest
   docker run -it --name joplin_server -v joplin:/home/joplin --env-file /home/waves/joplin/.env -p 22300:22300 joplin/server:latest
   
   docker run -it --name joplin_server -v joplin:/home/joplin --env-file /home/proton/joplin/.env -p 22300:22300 joplin/server:latest
   
   
   docker run -d --name joplin_server  --env-file /home/proton/joplin/.env -p 22300:22300 joplin/server:latest
   
   rm /etc/localtime
   
   ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
   
   setenv ethaddr 00:ab:00:11:22:33
   
   docker run -it --name joplin_server1 -v joplin:/home/joplin --env-file /home/waves/joplin/.env -p 22300:22300 joplin/server:latest
   ```

   ubuntu 运行容器
   
   ```bash
   docker run -it --name joplin_server -v joplin:/home/joplin --env-file /home/zsz/docker/joplin/.env -p 22300:22300 joplin/server:latest
   
   docker run -it --name joplin_server1 -v joplin:/home/joplin --env-file /home/zsz/docker/joplin/.env -p 22301:22300 joplin/server:latest
   
   ```
   

备注：

ipv6的80端口访问joplin是可以的

```bash
#APP_BASE_URL=http://localhost:22300
#APP_BASE_URL=http://192.168.1.102:22301
APP_BASE_URL=http://server.proton-core.xyz
APP_PORT=22300
# MAX_TIME_DRIFT=1000000
```

使用默认的sql数据库

```bash
#http://server.proton-core.xyz:22300也可以访问

#APP_BASE_URL=http://localhost:22300
#APP_BASE_URL=http://192.168.1.102:22301
APP_BASE_URL=http://server.proton-core.xyz:22300
APP_PORT=22300
# MAX_TIME_DRIFT=1000000
zsz@ubuntu-server:~/docker/joplin$

```

注意<font color='red'>开启代理后会导致域名访问</font>的<font color='red'>失败</font>

### 使用postgres 数据库

```bash
#使用docke-compose
docker-compose --file  ./docker-compose.yml up
#后台运行
docker-compose --file ./docker-compose.yml up -d


```



邮箱

对于自己编写的邮件发送程序，需要知道对应邮箱的[smtp](https://so.csdn.net/so/search?q=smtp&spm=1001.2101.3001.7020)服务器，下面列举了些部分邮箱及对应的smtp服务器和支持的协议

| 邮箱    | smtp服务器            | 支持的协议（可能有遗漏）    |
| ------- | --------------------- | --------------------------- |
| gmail   | smtp.gmail.com        | TLS/ STARTTLS（TLS）        |
| qq      | smtp.qq.com           | SSL/TLS/ STARTTLS（TLS）    |
| foxmail | smtp.exmail.qq.com    | SSL/TLS/ STARTTLS（TLS）    |
| outlook | smtp-mail.outlook.com | STARTTLS（TLS）             |
| 雅虎    | smtp.mail.yahoo.com   | TLS/STARTTLS（TLS）         |
| 网易163 | smtp.163.com          | SSL/TLS                     |
| hotmail | smtp.live.com         | STARTTLS（TLS）             |
| icloud  | smtp.mail.me.com      | STARTTLS（TLS）             |
| Yandex  | smtp.yandex.ru        | SSL/TLS/STARTTLS（SSL/TLS） |
| GMX     | smtp.gmx.com          | TLS/STARTTLS                |
| 新浪    | smtp.sina.com         | SSL/TLS/STARTTLS（SSL/TLS） |
| aol     | smtp.aol.com          | TLS／STARTTLS               |
| rediff  | smtp.rediffmail.com   | SSL/TLS/STARTTLS（SSL/TLS） |

------

### 相关邮箱配置说明：

对于[ssl](https://so.csdn.net/so/search?q=ssl&spm=1001.2101.3001.7020)/tls加密，使用465端口
 对于starttls 一般使用587端口

```bash
qq
qq账户登入所需要的密码不是平时登入使用的密码，需去账户下生成，具体路径如下：
进入邮箱账户–> 设置 –> 账户 –> 生成授权码
将上面生成的授权码作为登入密码即可。

```



### 反向代理

1. [参考的反向代理文章443](https://blog.csdn.net/u013568040/article/details/124027860)    [参考非443端口](https://blog.csdn.net/tiancityycf/article/details/121685698)

2. 需要两个地方的配置 

   nginx 的配置文件都在nginx文件夹中了
   
   ```bash
   sudo vim nginx.conf # 这个文件中的 #include /etc/nginx/conf.d/joplin_proxy3;
   								 # include /etc/nginx/conf.d/joplin_proxy443;
   								 #负责开启相应的代理服务
   sudo vim conf.d/joplin_proxy3    #这个代理的是446端口到22300  对应的 docker-compose.yml
   
   conf.d/joplin_proxy443 #这个代理的是443端口，这个端口可能是服务商没有开，手机无法登录 对应的docker-compose443.yml
   ```
   
   

ntp 与ntpdate 服务同步时间

centos

```sh
[proton@proton-pc joplin]$ ntpdate cn.pool.ntp.org
13 May 23:20:52 ntpdate[11119]: bind() fails: Permission denied
[proton@proton-pc joplin]$ sudo ntpdate pool.ntp.org
13 May 23:36:10 ntpdate[12406]: adjust time server 108.61.73.243 offset 0.075971 sec
```



joplin_server 容器的停止与启动

```sh
#启动 joplin_server 容器
docker start joplin_server 
#停止 joplin_server 容器
docker stop joplin_server 

#登陆joplinserver 
docker exec -it  joplin_server /bin/bash

#进入容器中正在运行的终端
docker attach joplin_server
```



