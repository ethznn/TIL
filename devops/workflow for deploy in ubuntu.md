# workflow for deploy in ubuntu



## 1. adding user in server

### 접속 후 Linux에 deploy 계정 만들어주기

```bash
sudo adduser deploy
sudo adduser deploy sudo
sudo gpasswd -a deploy sudo
passwd deploy # 비밀번호 변경

ubuntu@private_ip:~$ sudo vi /etc/sudoers 로 파일을 열어
# User privilege specification
root    ALL=(ALL:ALL) ALL
deploy  ALL=(ALL) NOPASSWD:ALL # 추가
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL) NOPASSWD:ALL # 변경

# 사용자 deploy 모든 터미널에서 모든 사용자의 권한으로 모든 명령을 실행할 수 있다.
```

이제 만들어준 아이디로 접속할 수 있게끔 하는 public_key를 복사해주자

```bash
ssh -i pem_key deploy(added user)@publicIPv4
```

처음 접속 시

```bash
ssh -i pem_key경로 ubuntu(초기 아이디)@publicIPv4 
```

로 접속 후 

```bash
ubuntu@private_ip:~$ ls -al 히면
.ssh 파일이 있고 .ssh/authorized_keys 가 있는데 이것으로 접속을 허용해주는 것이다.
여기서 파일의 권한을 확인

그래서 접속 허용 이 키를 복사하기 위해 우선 deploy 에 .ssh 폴더를 만들어준다.
ubuntu@private_ip:~$ sudo mkdir ../deploy/.ssh

그리고 authorized_keys 복사 여넣기
ubuntu@private_ip:~$ sudo cp .ssh/authorized_keys ../deploy/.ssh/authorized_keys

ubuntu@private_ip:/home/deploy$ ls -al 하면 .ssh 폴더가 있고 안에 들어가보면 키가 있다.

권한이 중요한데 이 readme.md 기준 ../server_client/chmod.md 파일 참고
				파일권한		소유자		소유그룹
내 경우  -rw------- ubuntu ubuntu authorized_keys 이였어서 deploy 계정으로 접속하기 위해서 

소유자 소유그룹을 모두 deploy로 바꿔주기로 했다.
ubuntu@private_ip:~$ sudo chown deploy.deploy ../deploy/.ssh/authorized_keys

```

그리고 접속을 끊은 후 

```bash
ssh -i pem_key경로 deploy(added user)@publicIPv4
```

로 해주면 접속 가능!!

### 추가로 자신의 로컬에서 어느 위치던 pem_key경로 를 적지 않고

```
ssh deploy(added user)@publicIPv4
```

로 접속하고 싶다면 로컬의 .ssh 에서 

```bash
vi config

후에 

Host publicIPv4
User deploy
IdentityFile pem_key경로

를 적어주고 :wq(저장)
```

그리고 어느 위치에서든 

```bash
ssh deploy(added user)@publicIPv4
```

로 자신이 만든 원격 컴퓨터로 접속 가능하다.



## 2. install tools

#### deploy 로 접속 후에 설치하는 것을 추천함. (나중에 다시 깔아야하는 귀찮음이 생길 수 있음.)

### package update

```bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs yarn

// 추가 설명
패키지 인덱스 인덱스 정보를 업데이트: apt-get은 인덱스를 가지고 있는데
이 인덱스는 /etc/apt/sources.list에 있습니다. 이곳에 저장된 저장소에서 사용할 패키지의 정보를 얻습니다. 
sudo apt-get update

설치된 패키지 업그래이드: 설치되어 있는 패키지를 모두 새버전으로 업그래이드 합니다.
sudo apt-get upgrade
```

### install ruby 2.5.0

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 2.5.0
rbenv global 2.5.0
ruby -v

gem install bundler

rbenv rehash
```

### git config

```bash
sudo ssh-keygen -t rsa -b 4096
Enter file in which to save the key (/root/.ssh/id_rsa): /home/USER/.ssh/id_rsa #USER

sudo cat .ssh/id_rsa.pub
ssh-rsa ~~~~~~ root@privateIP

내용 복사 후
git repository setting deploy key 에 저장

# 참고로
sudo chown deploy.deploy /home/deploy/.ssh/id_rsa # 권한 변경해줘야 github deploy로 읽기 가능
```

### database (ex. postgresql)

```bash
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib

sudo apt-get install libpq-dev

pg_config # check postgresql info 
```

### nginx

```bash
# 먼저 nginx
sudo add-apt-repository -y ppa:nginx/stable
sudo apt-get update

sudo apt-get -y install nginx

sudo nginx -s reload # nginx restart command
# /etc/nginx/sites-enabled/default 파일 생성 됨
```

### certbot 

```bash
# https 인증을 위해 certbot 설치하고 인증 받기

sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get -y install certbot # 중간에 email 
sudo apt-get install python-certbot-nginx # 덕분에 너무 신세계

sudo certbot --nginx -d example.com # 자신이 구매한 도메인

sudo openssl dhparam 2048 -out /etc/ssl/certs/dhparam.pem # 시간 오래 걸린답
```

> 여기서 /etc/nginx/sites-enabled/server_name 생성 후 수정 / default는 삭제 
> server_name 파일 예시
>
> ```sh
> upstream puma_서버_환경 {
>   server unix:/home/deploy/apps/내가만든서버/shared/tmp/sockets/puma.sock fail_timeout=0;
> }
> 
> server {
>   listen 80 default_server;
>   listen [::]:80 default_server;
>   server_name 내도메인;
> 
>   # Redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
>   return 301 https://$host$request_uri;
> }
> 
> server {
>   server_name 내도메인;
>   listen 443 ssl http2;
>   listen [::]:443 ssl http2;
> 
>   # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
>   ssl_certificate /etc/letsencrypt/live/내도메인/fullchain.pem;
>   ssl_certificate_key /etc/letsencrypt/live/내도메인/privkey.pem;
>   ssl_session_timeout 1d;
>   ssl_session_cache shared:SSL:50m;
>   ssl_session_tickets off;
> 
>   # Diffie-Hellman parameter for DHE ciphersuites, recommended 2048 bits
>   ssl_dhparam /etc/ssl/certs/dhparam.pem;
> 
>   # intermediate configuration. tweak to your needs.
>   ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
>   ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
>   ssl_prefer_server_ciphers on;
> 
>   # HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)
>   add_header Strict-Transport-Security max-age=15768000;
> 
>   # OCSP Stapling ---
>   # fetch OCSP records from URL in ssl_certificate and cache them
>   ssl_stapling on;
>   ssl_stapling_verify on;
> 
>   ## verify chain of trust of OCSP response using Root CA and Intermediate certs
>   #ssl_trusted_certificate /path/to/root_CA_cert_plus_intermediates;
> 
>   resolver 8.8.8.8 8.8.4.4;
> 
> 
>   server_tokens off;
>   add_header X-Content-Type-Options nosniff;
> 
>   client_max_body_size 4G;
>   keepalive_timeout 65;
> 
>   error_page 500 502 504 /500.html;
>   error_page 503 @503;
> 
>   server_name 내도메인;
>   root /home/deploy/apps/내가만든서버/current/public;
>   try_files $uri/index.html $uri @puma_서버_환경;
> 
>   location @puma_서버_환경 {
>     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>     proxy_set_header Host $http_host;
>     proxy_redirect off;
>     proxy_set_header X-Forwarded-Proto https;
> 
>     proxy_pass http://puma_서버_환경;
>     # limit_req zone=one;
>     access_log /var/log/nginx/서버_환경.access.log;
>     error_log /var/log/nginx/서버_환경.error.log;
>   }
> 
>   location ^~ /assets/ {
>     gzip_static on;
>     expires max;
>     add_header Cache-Control public;
>     add_header Access-Control-Allow-Origin *;
> 
>     if ($request_filename ~* ^.*?\.(eot)|(ttf)|(woff)|(svg)|(otf)$) {
>       add_header Access-Control-Allow-Origin *;
>     }
>   }
> 
>   location = /50x.html {
>     root html;
>   }
> 
>   location = /404.html {
>     root html;
>   }
> 
>   location @503 {
>     error_page 405 = /system/maintenance.html;
>     if (-f $document_root/system/maintenance.html) {
>       rewrite ^(.*)$ /system/maintenance.html break;
>     }
>     rewrite ^(.*)$ /503.html break;
>   }
> 
>   if ($request_method !~ ^(GET|HEAD|PUT|PATCH|POST|DELETE|OPTIONS)$ ) {
>     return 405;
>   }
> 
>   if (-f $document_root/system/maintenance.html) {
>     return 503;
>   }
> }
> ```



### ubuntu 안에서 deploy를 위한 작업은 대부분 완료했다.

### 이제 rails 서버에서 bundle exec cap environmnet deploy 