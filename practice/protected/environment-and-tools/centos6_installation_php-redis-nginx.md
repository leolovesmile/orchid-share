# Redis(4.0)
```
wget http://download.redis.io/releases/redis-4.0.0.tar.gz
mv /home/tscnhala/redis-4.0.0.tar.gz  /opt/
tar xzf redis-4.0.0.tar.gz
cd redis-4.0.0
make
cd src && make install
redis-server -v
```


# Nginx(1.14)
config yum repo according to  https://www.nginx.com/resources/wiki/start/topics/tutorials/install/ 
> [nginx]
> name=nginx repo
> baseurl=http://nginx.org/packages/centos/6/x86_64/
> gpgcheck=0
> enabled=1

```
yum --showduplicates list nginx
yum install nginx-1.14.0
nginx -v
```



# PHP(7.2.6）
if you want to install from source, see https://sharonsahadevan.com/how-to-install-php-7-2-using-source-centos-7/ and https://www.php.net/releases/
```
export http_proxy="http://192.168.52.3:3128"
# To install latest PHP 7, you need to add EPEL and Remi repository to your CentOS 6 system like so.
yum install epel-release
#  Now install yum-utils, a group of useful tools that enhance yum’s default package management features
yum install yum-utils
# In this step, you need to enable Remi repository using yum-config-manager utility
yum-config-manager --enable remi-php72
# yum install php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo 
yum install php
php -V 

# uninstall 
rpm -qa |grep php
rpm -e php-cli-5.3.3-49.el6.x86_64
```