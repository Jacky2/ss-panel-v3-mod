演示教程，所用系统为 CentOS 7 64，也可以使用别的系统，教程里所用的命令，替换成相应系统的命令即可。


本教程，仅供参考。



前台搭建



安装 redis


yum install epel-release -y       // 当前目录： /root

yum install redis -y


启动redis


systemctl start  redis


停止redis


systemctl stop redis


重启redis


systemctl restart redis


检查状态


systemctl status redis


开启开机启动


systemctl enable redis


关闭随机启动


systemctl disable redis


测试redis


redis-cli ping


看到PONG就是测试通过



yum install -y screen   //可以后台运行，防止远程操作意外中断


screen -S lnmp          //lnmp 是任意取的名字，在进行安装操作期间如果被中断，可以使用 screen -r lnmp 恢复 ,如果screen 状态为Attached 连不上，可以执行screen -D -r lnmp试试


输入MYSQL数据库的root密码（默认为root）,如：123456



安装虚拟机


cd  /data/www

下载前台代码


git clone -b new_master git@github.com:Jacky2/ss-panel-v3-mod.git

ls -al              //显示目录和隐藏文件及权限

cp -a ss-panel-ssr/. ./

ls -al  


配置一下nginx了


vi /usr/local/nginx/conf/vhost/www.yanml.top.conf 


如， 在你原先的 root 目录后面加上 /public
root /data/www/ss-panel-v3-mod/public;

把下面，这段代码，加入到下方有lialocation的字段，这些字段以location为开头，下方两个｛｝括号之间为一个完整定义。任意一处。

location / {  
    try_files $uri $uri/ /index.php$is_args$args;
}


重新加载 Nginx

service nginx reload

提示为 Reload service nginx...  done


新建数据库用户和导入

//复制并重命名为.config.php 
cp .config.php.example .config.php     

vi .config.php

修改以下几处，其他配置根据自己情况来调整

# 随便填
$System_Config['key'] = 'ssr';
# 随便填
$System_Config['appName'] = 'ssr';
# 填你的服务器域名或者IP
$System_Config['baseUrl'] = 'http://IP or 域名';
# 随便填
$System_Config['salt'] = '127-ssr';
$System_Config['authDriver'] = 'redis';
$System_Config['sessionDriver'] = 'redis';
$System_Config['cacheDriver'] = 'redis';

# database 数据库配置
$System_Config['db_driver'] = 'mysql';
# 用localhost 可能会报错
$System_Config['db_host'] = '127.0.0.1';
$System_Config['db_database'] = 'ssr';
$System_Config['db_username'] = 'ssr';
$System_Config['db_password'] = 'password';

# redis配置
$System_Config['redis_scheme'] = 'tcp';
$System_Config['redis_host'] = '127.0.0.1';
$System_Config['redis_port'] = '6379';
$System_Config['redis_database'] = '0';
$System_Config['redis_password']="password";




安装倚赖库


curl -sS https://getcomposer.org/installer | php

php composer.phar  install


添加管理员


php xcat createAdmin

Enter password for: 1@2.cn / 为 1@2.cn 添加密码 123456
Email: 1@2.cn, Password: 123456! Press [Y] to create admin..... 按下[Y]确认来确认创建管理员账户..... y
start create admin accountSuccessful/添加成功![root@iZj6c4x4a45qmc2k068tdbZ www.ahbb.ml]# 

即表示 添加 管理员 成功

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
                                 


到时，前台搭建完成，可以登录看看。