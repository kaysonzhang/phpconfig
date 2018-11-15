#使用EPEL方式安装nginx：

* sudo yum install epel-release
* yum update
* yum install nginx

* systemctl start nginx
* systemctl enable nginx

#如果以上方法不行，请执行以下方法

* rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
* yum install nginx -y


#2.安装MYSQL
* yum localinstall http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm

* yum install mysql-community-server

* //开启mysql
* service mysqld start

* //查看mysql的root账号的密码
* grep 'temporary password' /var/log/mysqld.log

* //登录mysql
* mysql -uroot -p

* //修改密码
* ALTER USER 'root'@'localhost' IDENTIFIED BY 'Kayson@398635';

* //修改root用户可远程登录
* GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Kayson@398635' WITH GRANT OPTION; 

* //刷新
* flush privileges;

#3.安装php

* rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

* rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

* //查看
* yum search php72w

* //更多模块请看http://www.cnblogs.com/hello-tl/p/9404655.html

#//安装php以及扩展
* yum install php72w php72w-fpm php72w-cli php72w-common php72w-devel php72w-gd php72w-pdo php72w-mysql php72w-mbstring php72w-bcmath php72w-mcrypt php72w-xml php72w-mysqlnd php72w-opcache php72w-soap php72w-pecl-imagick php72w-pecl-imagick-devel
* //开启服务
* service php-fpm start

* //修改/etc/nginx/nginx.conf

* //重启nginx
* service nginx restart

#4.安装redis
* yum install redis
* * //修改配置 
vi /etc/redis.conf
* //daemonize yes 后台运行
* //appendonly yes 数据持久化
* service redis start


#5.安装php-redis扩展
* //先装git
* yum install git

* //git下扩展
* cd /usr/local/src
* git clone https://github.com/phpredis/phpredis.git

* //安装扩展
* cd phpredis
* phpize

* //修改php配置
* vi /etc/php.ini  添加extension=redis.so

* //重启php
* service php-fpm restart

* 直接用pecl库安装扩展就好
* 链接地址https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown
* yum install php72w-pear
* yum install gcc-c++
* pecl install redis

* 安装composer
* curl -sS https://getcomposer.org/installer | php
* mv composer.phar /usr/local/bin/composer



#[nginx引起的问题]
最后想起来了nginx的日志，就开始翻看nginx的error.log，结果发现了相应的错误记录：4511#4511: *11 "/data/www/index.html" is forbidden (13: Permission denied), client: 192.168.152.1（以前没有重视过log，现在发现它真是个好东西），直接将错误信息粘贴到百度。

具体原因就是服务器的SELinux设置为了开启(enabled)状态。

具体的解决办法如下：

1.临时的解决办法，不需要重启服务器

直接执行下面的命令：

setenforce 0
这种方法我没有尝试，不知道有效性。

2.修改配置文件/etc/selinux/config

将配置文件中的SELINUX=enforcing修改为SELINUX=disabled即可，修改完成后需要重启下机器。

这种方法应该是有效的，至少在我的机器中是通过该方式解决了问题。




