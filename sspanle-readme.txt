��ʾ�̳̣�����ϵͳΪ CentOS 7 64��Ҳ����ʹ�ñ��ϵͳ���̳������õ�����滻����Ӧϵͳ������ɡ�


���̳̣������ο���



ǰ̨�



��װ redis


yum install epel-release -y       // ��ǰĿ¼�� /root

yum install redis -y


����redis


systemctl start  redis


ֹͣredis


systemctl stop redis


����redis


systemctl restart redis


���״̬


systemctl status redis


������������


systemctl enable redis


�ر��������


systemctl disable redis


����redis


redis-cli ping


����PONG���ǲ���ͨ��



yum install -y screen   //���Ժ�̨���У���ֹԶ�̲��������ж�


screen -S lnmp          //lnmp ������ȡ�����֣��ڽ��а�װ�����ڼ�������жϣ�����ʹ�� screen -r lnmp �ָ� ,���screen ״̬ΪAttached �����ϣ�����ִ��screen -D -r lnmp����


����MYSQL���ݿ��root���루Ĭ��Ϊroot��,�磺123456



��װ�����


cd  /data/www

����ǰ̨����


git clone -b new_master git@github.com:Jacky2/ss-panel-v3-mod.git

ls -al              //��ʾĿ¼�������ļ���Ȩ��

cp -a ss-panel-ssr/. ./

ls -al  


����һ��nginx��


vi /usr/local/nginx/conf/vhost/www.yanml.top.conf 


�磬 ����ԭ�ȵ� root Ŀ¼������� /public
root /data/www/ss-panel-v3-mod/public;

�����棬��δ��룬���뵽�·���lialocation���ֶΣ���Щ�ֶ���locationΪ��ͷ���·�������������֮��Ϊһ���������塣����һ����

location / {  
    try_files $uri $uri/ /index.php$is_args$args;
}


���¼��� Nginx

service nginx reload

��ʾΪ Reload service nginx...  done


�½����ݿ��û��͵���

//���Ʋ�������Ϊ.config.php 
cp .config.php.example .config.php     

vi .config.php

�޸����¼������������ø����Լ����������

# �����
$System_Config['key'] = 'ssr';
# �����
$System_Config['appName'] = 'ssr';
# ����ķ�������������IP
$System_Config['baseUrl'] = 'http://IP or ����';
# �����
$System_Config['salt'] = '127-ssr';
$System_Config['authDriver'] = 'redis';
$System_Config['sessionDriver'] = 'redis';
$System_Config['cacheDriver'] = 'redis';

# database ���ݿ�����
$System_Config['db_driver'] = 'mysql';
# ��localhost ���ܻᱨ��
$System_Config['db_host'] = '127.0.0.1';
$System_Config['db_database'] = 'ssr';
$System_Config['db_username'] = 'ssr';
$System_Config['db_password'] = 'password';

# redis����
$System_Config['redis_scheme'] = 'tcp';
$System_Config['redis_host'] = '127.0.0.1';
$System_Config['redis_port'] = '6379';
$System_Config['redis_database'] = '0';
$System_Config['redis_password']="password";




��װ������


curl -sS https://getcomposer.org/installer | php

php composer.phar  install


��ӹ���Ա


php xcat createAdmin

Enter password for: 1@2.cn / Ϊ 1@2.cn ������� 123456
Email: 1@2.cn, Password: 123456! Press [Y] to create admin..... ����[Y]ȷ����ȷ�ϴ�������Ա�˻�..... y
start create admin accountSuccessful/��ӳɹ�![root@iZj6c4x4a45qmc2k068tdbZ www.ahbb.ml]# 

����ʾ ��� ����Ա �ɹ�

crontab -e

## update date time
*/20 * * * * /usr/sbin/ntpdate pool.ntp.org > /dev/null 2>&1

## ssr sspanel
30 22 * * * /data/programs/php56/bin/php /data/web/sspanel/xcat sendDiaryMail
0 0 * * * /data/programs/php56/bin/php /data/web/sspanel/xcat dailyjob
*/1 * * * * /data/programs/php56/bin/php /data/web/sspanel/xcat checkjob

## restart php and nginx
0 1 * * * systemctl restart php56
0 1 * * * systemctl restart nginx

## restart ssr

0 2 * * * /data/programs/shadowsocks/stop.sh && /data/programs/shadowsocks/logrun.sh

## db backup

#0 3 * * * /data/sh/db_backup.sh
                                 


��ʱ��ǰ̨���ɣ����Ե�¼������