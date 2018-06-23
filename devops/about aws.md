# about AWS

#### related EC2

ec2 생성 후 탄력적 IP 사용해 끄고 킬때 따로 IP 변경을 신경쓰지 않을 수 있도록 함.

퍼블릭 IP 변경됨.

ssh 접속

```bash
$ ssh -i pem_key경로 ubuntu(초기 아이디)@publicIPv4
```





#### related IAM

개인이 아닌 회사나 그룹으로 함께 공유하는 계정의 경우

Root 사용자를 사용하는 것을 지양함. (모든 권한을 가진 상태로 어떤 구성원이 실수로 어떤 행동을 할 시에 치명적인 문제가 생길 수 있음)

그래서 사용자를 만들어 각 리소스에 대한 권한을 나눠주고 전담하게끔 구성.

1. AWS CLI profile 설정

```bash
$ aws configure
```

2. 새로운 user 추가 후 change another profile

```bash
$ aws configure --profile user2
$ export AWS_PROFILE=user2 # 환경 변수 변경으로 유저 변경

# AWS CLI는 다음과 같은 환경 변수를 지원합니다.
# AWS_ACCESS_KEY_ID - AWS 액세스 키.
# AWS_SECRET_ACCESS_KEY - AWS 보안 키. 액세스 및 보안 키 변수는 자격 증명 파일과 config 파일에 저장된 자격 증명을 재정의합니다.
# AWS_SESSION_TOKEN - 임시 보안 자격 증명을 사용하는 경우 세션 토큰을 지정합니다.
# AWS_DEFAULT_REGION - AWS 리전. 이 변수는 사용 중인 프로필의 기본 리전(설정된 경우)을 재정의합니다.
# AWS_DEFAULT_OUTPUT - AWS CLI의 출력 형식을 json, text 또는 table로 변경합니다.
# AWS_PROFILE - 사용할 CLI 프로필의 이름입니다. 이 이름은 자격 증명 파일 또는 config 파일에 저장된 프로필의 이름, 또는 기본 프로필을 사용할 default일 수 있습니다.
# AWS_CA_BUNDLE - HTTPS 인증서 확인에 사용할 인증서 번들의 경로를 지정합니다.
# AWS_SHARED_CREDENTIALS_FILE - AWS CLI에서 액세스 키를 저장하는 파일의 위치를 변경합니다.
#AWS_CONFIG_FILE - AWS CLI에서 구성 프로필을 저장하는 파일의 위치를 변경합니다.
```

