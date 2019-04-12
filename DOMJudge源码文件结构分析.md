---
title: DOMJudge（6.0.3）源码文件结构分析 
tags: DOMJudge,Source,Analysis
grammar_cjkRuby: true
---
[toc]
# domjudge
> DOMJudge源码文件结构如下：
- domjudge/
	- judgehost/
	- domserver/
	- doc/

## judgehost
>judgehost下的文件结构：
- domjudge/
	- judgehost/
		- bin/
		- etc/
		- judgings/
		- log/
		- run/
		- temp/
		- lib/
	- domserver
	- doc

### bin/

#### runguard 
负责保证用户程序的安全运行, 不会对系统进行破坏的程序, 这个的代码利用了 rlmit, cgroup timeout等方式对用户程序进行限制

#### judgedaemon 
一个php脚本, 测评后端的完整进程, 包括通过服务器下载数据, 到调用相应可执行文件对代码进行编译运行比对之后. 将结果返回给webserver的整个过程都是由这个程序进行的

#### runpipe:
用于在runjury(特殊的测评程序) 和 user program之间建立双向管道,进行交互式测评

#### create_cgroups:
在设置了enable_cgroup 的编译参数之后, 通过这个来创建domjudge的 control group, cgroup能够获得更精确的运行时间和使用资源情况

#### dj_run_chroot,dj_make_chroot, dj_make_ubuntu_chroot
配置chroot环境，保证能够编译和运行

### etc/

#### sudoers.domjudge
这个文件配置好了 运行测评的时候的用户的sudo权限配置

#### common-config.php，judgehost-config.php
一些通常的config文件,和本机的配置文件

#### judgehost-static.php
通过make 和 make install生成的静态config文件

#### restapi.secret:
里面存有向API请求的时候的API地址和认证信息

### judgings/

judgings文件夹下, 所有的文件都是测评的时候生成的
每个judgings文件夹下, 有多个endpoint, 每一个endpoint对应你再 restapi.secret里面配置的不同的名字

### log/
log文件夹内只存了必要的log信息, judgehost的运行日志，在控制台也有输出

### run/
运行过的源文件信息
### tmp/
临时存储，为空
### lib/judge
> lib/ 支持judgehost正常的运行的php文件的shell脚本，例如lib.error.php lib.error.sh
#### testcase_run.sh 
用于运行程序测评的脚本, 脚本接受多个参数 会在介绍judge流程的时候具体介绍

#### sh-static
一个静态的 shell

#### judgedaemon.main.php 
整个测评运行的主要程序, 实际上 bin下的judgedaemon就是来调用这个程序的

#### chroot-startstop.sh
处理chroot

#### check_diff.sh
简单的compare脚本

#### compile.sh
保证compile不超时, 不超资源
## domserver
>domserver下的文件结构
- domserver/
	-  bin/
	-  etc/
	-  lib/
	-  log/
	-  run/
	-  sql/
	-  submissions/
	-  tmp/
	-  webapp/  
	-  www/


### bin/  
#### balloons
Generated from 'balloons.in'的PHP脚本
通过气球守护进程来监控最新的正确答案并通知分发气球，这个守护进程和judgehost的守护进程不能同时使用。
?
#### dj_setup_database
建库和建表时使用的shell脚本
通常只在安装domjudge时使用

#### multi_rejudge
 Generated from 'multi_rejudge.in' 的PHP脚本
 这个脚本是judgehost上用于统计数据。
 
#### simulate_contest
Generated from 'simulate_contest.in' 的PHP脚本
模拟比赛的PHP脚本，可以对已举办过的比赛进行模拟

#### combined_scoreboard 
Generated from 'combined_scoreboard.in'的PHP脚本
它将从不同server上的积分排行榜整合成总的排行榜

#### eventdaemon
?由eventdaemon.in生成的PHP脚本
?比赛守护程序
 当比赛开始之后改守护程序能够保证比赛数据等不受更改
 
#### restore_sources2db save_sources2file
由restore_sources2db.in,save_sources2file.in生成的PHP脚本
当数据库崩溃时，可以使用restore文件重建数据库，从domjudge的文件系统上恢复数据,而后一个文件则是将提交的数据都保存成文件

#### create_accounts  
由 create_accounts.in生成的PHP脚本
为每个用户创建相同名称的团队帐户，输入两个变量，用户名和密码

#### judging_statistics
Generated from 'judging_statistics.in'的PHP脚本
为提交的源码的判断集生成统计数据



### etc/  
>domjudge系统的配置文件：

#### common-config.php 
domjudge的总配置文件也包含了其他特殊配置文件
#### apache.conf nginx-conf 
apahce/nginx配置文件
#### domserver-config.php domserver-static.php      
domserver配置文件
#### dbpasswords.secret gendbpasswords        
数据库密码配置文件和生成数据库密码的脚本
#### restapi.secret genrestapicredentials
REST API密码配置文件和生成REST API的脚本
#### domjudge-fpm.conf  
domjudge php 配置文件
#### import-forwardfeed.yaml
第一次make domserver的时候导入的配置 包括默认网址，管理员账户密码等

### lib/  
>domjudge共享的库文件:

#### alert
调用domjudge库文件时的通知消息

#### lib.dbconfig.php   lib.database.php   relations.php    use_db.php
对数据库操作的库，包含数据库配置文件，数据库读写文件，数据库表名

#### init.php      lib.error.php     lib.timer.php 
初始化domjudge函数调用库，以及基本的函数库

#### vendor/
PHP包管理工具composer生成的文件夹

#### lib.impexp.php submit/  
提交的源文件导入，导入文件使用的php脚本和对应的文件夹

####  lib.wrappers.php  lib.misc.php      www/
  cookie以及domserver web的php文件

### log/  run/  tmp/
日志和临时存储的文件夹

### sql/
初始为demo contest 的sql 文件
当运行 restore_sources2db save_sources2file后，就是重建数据库所保存的sql文件

### submissions/  
该文件夹下包含提交的源码文件，并重命名

### webapp/  www/
> webapp存放了domjudge系统web端入口以及资源文件

- - webapp/
	- app  
	- bin 
	- phpunit.xml.dist  
	- src  
	- tests  demo contest 
	- var  
	- web 

> www存放domjudge api jury public team 界面以及相应的PHP文件

- - www/
	-   api  
	-   auth
	-   configure.php  
	-   feed  
	-   index.php  
	-   jury  
	-   public  
	-   team

## doc/
>一些简单的文档说明，比如MIT，country_codes.txt。
>还有一部分是官网的文档说明
>Changlog是版本变更说明
>以下是doc下目录结构
- doc/
	- admin/
	- examples/
	- team/
		- gentexconfig
	- judge/
	- logos/


### admin/
官方admin文档的pdf版本和html版本

### examples/
demo contest的问题文档和示例可通过的源码

 
### team/

包含了官方的team手册和团队配置文件

 #### gentexconfig
 这个脚本用TeX格式输出团队配置设置，可以从这个文件恢复配置设置。
 

### judge/
官方judge文档的pdf版本和html版本

 ### logos/
 logos资源文件png、需要替换图标在这里替换，并替换软连接