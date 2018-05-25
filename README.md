# nginx-conf

```
* nginx 다운로드
https://nginx.org/en/download.html

* 엔진엑스 컴파일 설정

* 엔진엑스 기존 컴파일 설정을 확인하고 싶을 때
$ nginx -V // V 대문자

* Docker 컨테이너에서 nginx 컴파일 하기

docker 에서 local-centos-nginx 이름으로 centOS 6.8 컨테이너 띄우기
$ docker run -itd --name local-centos-nginx centos:6.8 bash

docker 에서 local-centos-nginx 컨테이너 쉘로 들어가기
$ docker attach local-centos-nginx

docker 컨테이너 쉘에서 nginx 디렉토리 만들기
[root@1ccbcb628763 /]# mkdir nginx

docker 컨테이너에서 잠시 나오기
ctrl 을 누른 상태에서 p 1번 q 1번 누르기

docker 컨테이너로 파일 복사하기
$ docker cp nginx-1.14.0.tar.gz local-centos-nginx:/nginx/

다시 docker 에서 local-centos-nginx 컨테이너 쉘로 들어가기
$ docker attach local-centos-nginx

docker 컨테이너에서 nginx 폴더 들어가서 압축 해제
[root@1ccbcb628763 /]# tar -xvzf nginx-1.14.0.tar.gz

docker 컨테이너에서 nginx 압축 해제 폴더로 이동
[root@1ccbcb628763 /]# cd nginx-1.14.0

nginx 컴파일할 계정(에, partner) 생성 아래 두 줄에서 택1
[root@1ccbcb628763 /]# useradd -m partner // -m 옵션으로 home 디렉토리에 생성된 계정 명으로 홈 디렉토리 자동 생성
[root@1ccbcb628763 /]# useradd -d /users/partner partner // -d 옵션으로 지정된 경로를 홈 디렉토리로 자동 생성
[root@1ccbcb628763 /]# su partner // partner 계정으로 로그인
[partner@1ccbcb628763 nginx-1.14.0]$ groups // 본인의 그룹명 확인
partner
[partner@1ccbcb628763 nginx-1.14.0]# exit // partner 계정에서 root 계정으로 나가기
[root@1ccbcb628763 /]# cd .. && cd nginx && cd nginx-1.14.0 // nginx compile 폴더로 이동
[root@1ccbcb628763 nginx-1.14.0]# 

docker 컨테이너에서 gcc 컴파일러 설치
[root@1ccbcb628763 nginx-1.14.0]# yum install gcc

docker 컨테이너에서 pcre 컴파일러 설치
[root@1ccbcb628763 nginx-1.14.0]# yum install pcre-devel

docker 컨테이너에서 openssl 컴파일러 설치
[root@1ccbcb628763 nginx-1.14.0]# yum install openssl-devel

docker 컨테이너에서 configure 환경 설정
[root@1ccbcb628763 nginx-1.14.0]# ./configure --prefix=/home/partner/nginx --user=partner --group=partner --with-http_ssl_module --with-http_v2_module --with-stream --with-stream_ssl_module

docker 컨테이너에서 nginx compile
[root@1ccbcb628763 nginx-1.14.0]# make install

docker 컨테이너에서 nginx compile 폴더로 이동
[root@1ccbcb628763 nginx-1.14.0]# cd ../../home/partner/
[root@1ccbcb628763 partner]# ls
nginx

docker 컨테이너에서 nginx 폴더 압축
[root@1ccbcb628763 partner]# tar -cvzf nginx.tar.gz ./nginx/

docker 컨테이너에서 잠시 나온 후 현재 폴더로 가지고 오기
ctrl 을 누른 상태에서 p 1번 q 1번 누르기
$ docker cp local-centos-nginx:/home/partner/nginx.tar.gz ./

docker 컨테이너 종료
$ docker attach local-centos-nginx // 다시 컨테이너 접속
[root@1ccbcb628763 partner]# exit // 컨테이너 종료
exit

설치 서버로 sftp 로 접속 및 파일 업로드
sftp> put nginx.tar.gz

설치 서버에서 접속 해제 src 폴더로 압축 해제
$ tar -xvzf nginx.tar.gz

루트폴더에서
vi .bashrc 입력 후 아래 내용 입력
PATH=$PATH:/home/partner/nginx/sbin
위 내용 입력 후 빠져 나오기

$ source .bashrc // source 등록

/home/partner/nginx/conf/nginx.conf 설정 후
$ nginx -t // nginx 테스트, 아래와 같이 나오면 성공
nginx: the configuration file /home/partner/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /home/partner/nginx/conf/nginx.conf test is successful
$ nginx // nginx 실행
$ ps -ef | grep nginx // nginx 작동 확인
partner  102323      1  0 08:31 ?        00:00:00 nginx: master process nginx
partner  102324 102323  0 08:31 ?        00:00:00 nginx: worker process
partner  102332  97361  0 08:31 pts/1    00:00:00 grep nginx
```

```
* configure 설명
--prefix= :  설치 경로
--user=nginx : 안넣으면 컴파일한 계정으로 자동을 들어
--group=nginx : 안넣으면 컴파일한 계정으로 자동을 들어감
--with-http_ssl_module :  SSL 인증서 모듈
--with-http_v2_module  : HTTP V2 지원 모듈
--with-stream  : TCP 연결을 위한 모듈
--with-stream_ssl_module : TCP 연결시 SSL 로 접속 할 수 있게 하는 모듈
그 밖에 선택적으로 넣으면 좋은 모듈 설정
--with-file-aio
--with-threads
--with-ipv6
--with-http_addition_module
--with-http_auth_request_module
--with-http_dav_module
--with-http_flv_module
--with-http_gunzip_module
--with-http_gzip_static_module
--with-http_mp4_module
--with-http_random_index_module
--with-http_realip_module
--with-http_secure_link_module
--with-http_slice_module 
--with-http_stub_status_module 
--with-http_sub_module 
--with-mail 
--with-mail_ssl_module
```

```
* production1 nginx-V
nginx version: nginx/1.12.1
built by gcc 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4)
built with OpenSSL 1.0.2g  1 Mar 2016
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-file-aio --with-threads --with-ipv6 --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_ssl_module --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie' --add-module=/home/partner/src/ModSecurity-nginx
```