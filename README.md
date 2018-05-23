# nginx-conf

```
* nginx 다운로드
https://nginx.org/en/download.html

* 엔진엑스 컴파일 설정

* 엔진엑스 기존 컴파일 설정을 확인하고 싶을 때
$ nginx -V // V 대문자

* Docker 컨테이너에서 nginx 컴파일 하기

docker 에서 local-centos-nginx 이름으로 centOS 6.8 컨테이너 띄우기
docker run -itd --name local-centos-nginx centos:6.8 bash

docker 에서 local-centos-nginx 컨테이너 쉘로 들어가기
docker attach local-centos-nginx

docker 컨테이너 쉘에서 nginx 디렉토리 만들기
mkdir nginx

docker 컨테이너에서 잠시 나오기
ctrl 을 누른 상태에서 p 1번 q 1번 누르기

docker 컨테이너로 파일 복사하기
docker cp nginx-1.14.0.tar.gz local-centos-nginx:/nginx/

다시 docker 에서 local-centos-nginx 컨테이너 쉘로 들어가기
docker attach local-centos-nginx

docker 컨테이너에서 nginx 폴더 들어가서 압축 해제
tar -xvzf nginx-1.14.0.tar.gz

docker 컨테이너에서 nginx 압축 해제 폴더로 이동
cd nginx-1.14.0
```