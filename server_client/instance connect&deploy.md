### aws 서버 접속하기 pem key 이용

접속할 때

```bash
sudo ssh -i pem경로/pem파일 user@아이피
```



필요시 vi ~/.ssh/config 에서

```
Host IPv4
User user (내 경우 대부분 root)
IdentityFile ~/.ssh/pem파일
```

:wq (저장) 하면



접속할 때

```bash
sudo ssh User(내 경우 root)@IPv4
```

하면 접속 가능하다.



위 설정 후 자신의 로컬에서

```bash
bundle exec cap production deploy
```

하면 자동으로 github에 있는 master branch 의 코드를 가져와 deploy를 함.