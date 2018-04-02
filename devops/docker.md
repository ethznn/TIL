# docker usage

docker container 생성

```bash
$ docker run -p 8080:8080 --name=이름 -v $(which docker):/bin/docker -v /var/run/docker.sock:/var/run/docker.sock -v 로컬안에연결할path:복사해놓을path:rw --env-file 환경변수저장파일path -d 가져올이미지
```



docker container 	보기 (container ID 확인)

```bash
$ docker ps // 사용 가능 container 보기
$ docker ps -al // 모든 container 보기
```



docker container 연결

```bash
$ docker exec -it container_id bash
```



docker container 	삭제

```bash
$ docker stop container_id // 중지가 안 되어있다면 중지 먼저
$ docker rm container_id
```

