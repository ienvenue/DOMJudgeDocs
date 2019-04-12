---
title: DOMJudge��6.0.3��Դ���ļ��ṹ���� 
tags: DOMJudge,Source,Analysis
grammar_cjkRuby: true
---
[toc]
# domjudge
> DOMJudgeԴ���ļ��ṹ���£�
- domjudge/
	- judgehost/
	- domserver/
	- doc/

## judgehost
>judgehost�µ��ļ��ṹ��
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
����֤�û�����İ�ȫ����, �����ϵͳ�����ƻ��ĳ���, ����Ĵ��������� rlmit, cgroup timeout�ȷ�ʽ���û������������

#### judgedaemon 
һ��php�ű�, ������˵���������, ����ͨ����������������, ��������Ӧ��ִ���ļ��Դ�����б������бȶ�֮��. ��������ظ�webserver���������̶��������������е�

#### runpipe:
������runjury(����Ĳ�������) �� user program֮�佨��˫��ܵ�,���н���ʽ����

#### create_cgroups:
��������enable_cgroup �ı������֮��, ͨ�����������domjudge�� control group, cgroup�ܹ���ø���ȷ������ʱ���ʹ����Դ���

#### dj_run_chroot,dj_make_chroot, dj_make_ubuntu_chroot
����chroot��������֤�ܹ����������

### etc/

#### sudoers.domjudge
����ļ����ú��� ���в�����ʱ����û���sudoȨ������

#### common-config.php��judgehost-config.php
һЩͨ����config�ļ�,�ͱ����������ļ�

#### judgehost-static.php
ͨ��make �� make install���ɵľ�̬config�ļ�

#### restapi.secret:
���������API�����ʱ���API��ַ����֤��Ϣ

### judgings/

judgings�ļ�����, ���е��ļ����ǲ�����ʱ�����ɵ�
ÿ��judgings�ļ�����, �ж��endpoint, ÿһ��endpoint��Ӧ���� restapi.secret�������õĲ�ͬ������

### log/
log�ļ�����ֻ���˱�Ҫ��log��Ϣ, judgehost��������־���ڿ���̨Ҳ�����

### run/
���й���Դ�ļ���Ϣ
### tmp/
��ʱ�洢��Ϊ��
### lib/judge
> lib/ ֧��judgehost���������е�php�ļ���shell�ű�������lib.error.php lib.error.sh
#### testcase_run.sh 
�������г�������Ľű�, �ű����ܶ������ ���ڽ���judge���̵�ʱ��������

#### sh-static
һ����̬�� shell

#### judgedaemon.main.php 
�����������е���Ҫ����, ʵ���� bin�µ�judgedaemon������������������

#### chroot-startstop.sh
����chroot

#### check_diff.sh
�򵥵�compare�ű�

#### compile.sh
��֤compile����ʱ, ������Դ
## domserver
>domserver�µ��ļ��ṹ
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
Generated from 'balloons.in'��PHP�ű�
ͨ�������ػ�������������µ���ȷ�𰸲�֪ͨ�ַ���������ػ����̺�judgehost���ػ����̲���ͬʱʹ�á�
?
#### dj_setup_database
����ͽ���ʱʹ�õ�shell�ű�
ͨ��ֻ�ڰ�װdomjudgeʱʹ��

#### multi_rejudge
 Generated from 'multi_rejudge.in' ��PHP�ű�
 ����ű���judgehost������ͳ�����ݡ�
 
#### simulate_contest
Generated from 'simulate_contest.in' ��PHP�ű�
ģ�������PHP�ű������Զ��Ѿٰ���ı�������ģ��

#### combined_scoreboard 
Generated from 'combined_scoreboard.in'��PHP�ű�
�����Ӳ�ͬserver�ϵĻ������а����ϳ��ܵ����а�

#### eventdaemon
?��eventdaemon.in���ɵ�PHP�ű�
?�����ػ�����
 ��������ʼ֮����ػ������ܹ���֤�������ݵȲ��ܸ���
 
#### restore_sources2db save_sources2file
��restore_sources2db.in,save_sources2file.in���ɵ�PHP�ű�
�����ݿ����ʱ������ʹ��restore�ļ��ؽ����ݿ⣬��domjudge���ļ�ϵͳ�ϻָ�����,����һ���ļ����ǽ��ύ�����ݶ�������ļ�

#### create_accounts  
�� create_accounts.in���ɵ�PHP�ű�
Ϊÿ���û�������ͬ���Ƶ��Ŷ��ʻ������������������û���������

#### judging_statistics
Generated from 'judging_statistics.in'��PHP�ű�
Ϊ�ύ��Դ����жϼ�����ͳ������



### etc/  
>domjudgeϵͳ�������ļ���

#### common-config.php 
domjudge���������ļ�Ҳ�������������������ļ�
#### apache.conf nginx-conf 
apahce/nginx�����ļ�
#### domserver-config.php domserver-static.php      
domserver�����ļ�
#### dbpasswords.secret gendbpasswords        
���ݿ����������ļ����������ݿ�����Ľű�
#### restapi.secret genrestapicredentials
REST API���������ļ�������REST API�Ľű�
#### domjudge-fpm.conf  
domjudge php �����ļ�
#### import-forwardfeed.yaml
��һ��make domserver��ʱ��������� ����Ĭ����ַ������Ա�˻������

### lib/  
>domjudge����Ŀ��ļ�:

#### alert
����domjudge���ļ�ʱ��֪ͨ��Ϣ

#### lib.dbconfig.php   lib.database.php   relations.php    use_db.php
�����ݿ�����Ŀ⣬�������ݿ������ļ������ݿ��д�ļ������ݿ����

#### init.php      lib.error.php     lib.timer.php 
��ʼ��domjudge�������ÿ⣬�Լ������ĺ�����

#### vendor/
PHP��������composer���ɵ��ļ���

#### lib.impexp.php submit/  
�ύ��Դ�ļ����룬�����ļ�ʹ�õ�php�ű��Ͷ�Ӧ���ļ���

####  lib.wrappers.php  lib.misc.php      www/
  cookie�Լ�domserver web��php�ļ�

### log/  run/  tmp/
��־����ʱ�洢���ļ���

### sql/
��ʼΪdemo contest ��sql �ļ�
������ restore_sources2db save_sources2file�󣬾����ؽ����ݿ��������sql�ļ�

### submissions/  
���ļ����°����ύ��Դ���ļ�����������

### webapp/  www/
> webapp�����domjudgeϵͳweb������Լ���Դ�ļ�

- - webapp/
	- app  
	- bin 
	- phpunit.xml.dist  
	- src  
	- tests  demo contest 
	- var  
	- web 

> www���domjudge api jury public team �����Լ���Ӧ��PHP�ļ�

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
>һЩ�򵥵��ĵ�˵��������MIT��country_codes.txt��
>����һ�����ǹ������ĵ�˵��
>Changlog�ǰ汾���˵��
>������doc��Ŀ¼�ṹ
- doc/
	- admin/
	- examples/
	- team/
		- gentexconfig
	- judge/
	- logos/


### admin/
�ٷ�admin�ĵ���pdf�汾��html�汾

### examples/
demo contest�������ĵ���ʾ����ͨ����Դ��

 
### team/

�����˹ٷ���team�ֲ���Ŷ������ļ�

 #### gentexconfig
 ����ű���TeX��ʽ����Ŷ��������ã����Դ�����ļ��ָ��������á�
 

### judge/
�ٷ�judge�ĵ���pdf�汾��html�汾

 ### logos/
 logos��Դ�ļ�png����Ҫ�滻ͼ���������滻�����滻������