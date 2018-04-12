# Deploy 따라하기

#### 1.Beanstalk 생성

AWS Elastic Beanstalk는 Java, [.NET](https://aws.amazon.com/ko/net/), PHP, Node.js, Python, Ruby, Go, [Docker](https://aws.amazon.com/ko/docker/)를 사용하여 Apache, Nginx, Passenger, [IIS](https://aws.amazon.com/ko/windows/)와 같은 친숙한 서버에서 개발된 웹 애플리케이션 및 서비스를 간편하게 배포하고 확장할 수 있는 서비스입니다.
코드를 업로드하기만 하면 Elastic Beanstalk가 용량 프로비저닝, 로드 밸런싱, 자동 크기 조정부터 시작하여 애플리케이션 상태 모니터링에 이르기까지 배포를 자동으로 처리합니다. 이뿐만 아니라 애플리케이션을 실행하는 데 필요한 AWS 리소스를 완벽하게 제어할 수 있으며 언제든지 기본 리소스에 액세스할 수 있습니다.
Elastic Beanstalk는 추가 비용 없이 애플리케이션을 저장 및 실행하는 데 필요한 AWS 리소스에 대해서만 요금을 지불하면 됩니다.



대쉬보드에서 Elastic Beanstalk선택.

새 애플리케이션 생성에서 애플리케이션 이름과 설명을 선택하고 다음 클릭

새 환경에서 웹서버 환경 선택

환경 유형에서 PHP와 단일 인스턴스 선택
http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/php-v1.zip 에서 PHP 샘플 파일을 다운로드

다운로드한 php-v1.zip 파일을 선택.

환경 이름에 고유한 이름을 기재

VPC 내부에 생성 선택

구성 세부 정보를 설정에 맞게 구성

서브넷 선택

이후 Elastic Beanstalk 생성

생성이 완료된후 환경 URL을 이용해 사이트에 접속가능



 

#### 2.생성된 Application Update

1)앞서 받은 php-v1.zip파일을 압축을 풀어 index.php파일을 메모장으로 연다.

2) index.php파일의 <h1> 태그 부분 원하는 단어로 수정.

3)파일을 v2로 변경하여 업데이트

4)해당 애플리케이션을 선택 후 애플리케이션 버전 클릭

5)새로 생성된 php-v2.zip파일을 업로드

6) php-v2버전을 선택후 배포 클릭 
7) 배포가 완료된 후 환경 URL로 접속하면 변경된 화면에 접속!