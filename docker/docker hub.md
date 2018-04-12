# usage of docker hub 

### command

1. download images

   ```bash
   $ docker pull [option] <image_name>[:tag]
   $ docker pull [option] <url>
   ```

   tag 붙이지 않으면 자동으로 최신 버전으로

2. show image list

   ```bash
   $ docker images [option] [reposiory_name]
   ```

3. show image detail

   ```bash
   $ docker inspect [option] <container&image_name,ID>
   ```

   - Image ID
   - 생성일
   - Docker version
   - Image creator
   - CPU

4. config of docker image tag

   ```bash
   $ docker tag <image>[:tag] <docker hub name(prxyeo)>/<image_name>[:tag]
   ```

5. search docker image

   ```bash
   $ docker search [option] <search_keyword>
   ```

   | 옵션             | 설명                   |
   | ---------------- | ---------------------- |
   | –automated=false | Automated Build만 표시 |
   | –no-trunc=false  | 모든 결과 표시         |
   | -s[–stars=0]     | 특정 개수 이상의 별 수 |

   | 항목        | 설명                                          |
   | ----------- | --------------------------------------------- |
   | NAME        | Docker Image명                                |
   | DESCRIPTION | Docker Image 설명                             |
   | STARS       | 해당 이미지가 받은 별 수                      |
   | OFFICIAL    | 공식 이미지 여부                              |
   | AUTOMATED   | Dockerfile을 기반으로 자동 생성된 이미지 여부 |

6. remove docker image

   ```bash
   $ docker rmi [option] <image_name>
   ```

   | 옵션             | 설명                                    |
   | ---------------- | --------------------------------------- |
   | -f, –force=false | 이미지 강제 삭제                        |
   | –no-prune=false  | 태그가 없는 부모 이미지를 삭제하지 않음 |

7. docker hub login

   ```bash
   $ docker login [option] [server_name]
   ```

   | 옵션                   | 설명                      |
   | ---------------------- | ------------------------- |
   | -u, –username=""       | 사용자명 -p, –password="" |
   | 패스워드 -e, –email="" | 이메일 주소               |

   ```bash
   $ docker login
   Login with your Docker ID to push and pull images from Docker Hub. If you dont have a Docker ID, head over to https://hub.docker.com to create one.
   Username: 사용자명
   Password: 패스워드
   Login Succeeded
   ```

8. upload docker image

   ```bash
   $ docker push <image_name>[:tag]
   ```

   ```bash
   $ docker push nuti0102/ubuntu:1.0
   The push refers to a repository [docker.io/nuti0102/ubuntu]
   0d45be5b95d8: Mounted from library/ubuntu
   18568efa7ad4: Mounted from library/ubuntu
   1c53295311c1: Mounted from library/ubuntu
   dfcc17ddae9e: Mounted from library/ubuntu
   d29d52f94ad5: Mounted from library/ubuntu
   1.0: digest: sha256:3b64c309deae7ab0f7dbdd42b6b326261ccd6261da5d88396439353162703fb5 size: 1357
   ```

9. docker hub logout

   ```bash
   $ docker logout [server_name]
   ```

   ```bash
   $ docker logout
   Remove login credentials for https://index.docker.io/v1/
   ```

