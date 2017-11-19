# 나만의 스트리밍 서버 만들기



## Introduction



Nginx + RTMP + ffmpeg를 활용한 미디어 서버



## Preparations



OS : Cent os 7.3 - 1611 

Kernel : 3.10.0-514

IP : 공인 IP

SSL : 서버와 웹 클라이언트 간 통신시 암호를 위해서 준비

Source : home / 계정 ID / Src에 임시 저장



## Setup & Configuration



1.  Nginx repo Add

```
vi /etc/yum.repos.d/nginx.repo
```

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/package/centos/7/$basearch // nginx.org/package/OS/OSRELEASE/$basearch
gpgcheck=0
enable=1
```

2.  Nginx setup

```
yum install -y nginx
```

3.  Firewall Configuration and Nginx execute

```
firewall -cmd --permanent --zone=public  --add-service=http // http open
firewall -cmd --permanent --zone=public  --add-service=https // https open
firewall -cmd --reload

systemctl start nginx
systemctl enable nginx // when computer boot, auto start
```

4. Nginx-rtmp-module setup

```
wget https://github.com/arut/nginx-rtmp-module/archive/master.zip
unzip master.zip
```

5. ffmpeg setup

```
wget http://ffmpeg.org/release/ffmpeg-3.4.tar.bz2 // find version in http://ffmpeg.org
tar -xvjf ffmpeg-3.4.tar.bz2
wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz // recently releases version 1.3.0
tar -zxvf yasm-1.3.0.tar.gz

cd yasm-1.3.0 // yasm configure, install
make
make install

cd ffmpeg-3.4 // ffmpeg configure, install
./configure 
make
make install

// if finish all setup, make test case
ffmpeg -i test.mp3 -f wav test.wav // -i filename.mp3 -f wav filename.wav

// If you want to install more codecs, See https://trac.ffmpeg.org/wiki/CompilationGuide/Centos
```

