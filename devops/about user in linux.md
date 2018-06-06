# adding user in server

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

로 접속하고 싶다면

로컬의 .ssh 에서 

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