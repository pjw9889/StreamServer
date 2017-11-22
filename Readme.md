나만의 스트리밍 서버 만들기



## Introduction



Nginx + RTMP + ffmpeg를 활용한 미디어 서버



## Preparations



OS : Cent os 7.3 - 1611 

Kernel : 3.10.0-514

IP : 공인 IP

SSL : 서버와 웹 클라이언트 간 통신시 암호를 위해서 준비

Source : /src에 저장



nginx : http://nginx.org/



nginx-rtmp

```
git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module  
```



openssl

```
https://www.openssl.org/source/
```



ffmpeg

```
wget https://raw.githubusercontent.com/q3aql/ffmpeg-install/master/ffmpeg-install
```



## Setup & Configuration

1.  Nginx Setup



```
tar -zxvf nginx-1.12.2.tar.gz
yum install -y pcre* zlib*
./configure --prefix=/etc/nginx --add-module=/src/nginx-rtmp-module --with-openssl=/src/openssl-1.0.2m --with-http_ssl_module --with-debug --with-http_sub_module
make
make install
```



2. cors issue 해소를 위해 /etc/nginx에 crossdomain.xml 추가

```
== crossdomain.xml ==
<?xml version="1.0" ?>
<cross-domain-policy>
<allow-access-from domain="*" />
</cross-domain-policy>
```



3.  Make /etc/init.d/nginx

```
vi /etc/init.d/nginx // Upload additional content
sed -i -e 's/\r//g' /etc/init.d/nginx
```



4. Firewall Configuration and Nginx execute

```
firewall-cmd --permanent --zone=public  --add-service=http // http open
firewall-cmd --permanent --zone=public  --add-service=https // https open
firewall-cmd --reload

systemctl start nginx
chkconfig name on // when computer boot, auto start
```

