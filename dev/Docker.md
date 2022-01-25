# Docker

- 리눅스 컨테이서 사용
- 호스트와 OS 자원(시스템 콜) 공유
- Immutable Infrastructure

### 실행
````
docker + <명령>
````

- 이미지 검색
````
docker search <이미지 이름>
````

- 이미지 받기
  - latest: 최신 버전
  - 사용자명/이미지 이름: 해당 사용자가 올린 이미지
````
docker pull <이미지 이름> : <태그>
````

- 이미지 목록
````
docker images
````
````
docker images <이미지 이름>
````

- 컨테이너 생성
````
docker run <옵션> <이미지 이름> <실행할 파일>
````
- ex) 이미지 생성 후 bash 셀 실행
  - "-i": interactive
  - "-t": pseudo--tty
  - "-i -t": bash 셀에 입력 출력
  - "--name": 컨테이름 이름 지정
````
docker run -i -t --name hello ubuntu /bin/bash
````

- 컨테이너 목록
  - 실행 컨테이너만 출력
````
docker ps
````
  - 정지 컨테이너 포함 출력
````
docker ps -a
````

- 컨테이너 시작
````
docker start <컨테이너 이름>
````

- 컨테이너 재시작
````
docker restart <컨테이너 이름>
````

- 컨테이너 접속
````
docker attach <컨테이너 이름>
````

- 컨테이너 안 명령 실행
````
docker exec <컨테이너 이름> <명령> <매개 변수>
````

- 컨테이너 내의 파일을 호스트로 복사
````
docker cp <컨테이너 이름>:<컨테이너 내의 파일경로> <호스트 디렉터리 경로>
docker cp <호스트 파일> <컨테이너 이름>:<컨테이너 내의 파일 경로>
````

- 컨테이너 정지
````
docker stop <컨테이너 이름>
````

- 컨테이너 삭제
````
docker rm <컨테이너 이름>
````

- 이미지 삭제
````
docker rmi <이미지 이름>:<태그>
````

### 옵션
````
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
````
````
-d	detached mode 흔히 말하는 백그라운드 모드
-p	호스트와 컨테이너의 포트를 연결 (포워딩)
-v	호스트와 컨테이너의 디렉토리를 연결 (마운트)
-e	컨테이너 내에서 사용할 환경변수 설정
–name	컨테이너 이름 설정
–rm	프로세스 종료시 컨테이너 자동 제거
-it	-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션
–link	컨테이너 연결 [컨테이너명:별칭]
````
````
--add-host=[호스트명:IP address] 컨테이너의 /etc/hosts에 호스트명과 IP Address 설정
--dns=[IP address] dns 서버의 IP Address 설정
--expose=[포트 번호] 포트 번호 할당
--mac--address=[MAC Address] 컨테이너의 Mac Address 설정
--net=[bridge|none|container:<name|d>|host] 컨테이너 네트워크 설정
-h, --hostname="호스트명" 컨테이너 호스트명 설정
-P, --publish-all=[true|false] 임의의 포트를 컨테이너에 할당
-p [호스트 포트번호]:[컨테이너 포트 번호] 호스트와 컨테이너의 포트 매핑
--link=[컨테이너명:alias] 다른 컨테이너에서 액세스 시 이름 설정
````
### Dockerfile
- 이미지 설정 파일
- docker 컨테이너 안에서 실행하는 커맨드
- 환경변수 설정
````
FROM 베이스 이미지 지정
MAINTAINER Dockerfile 생성자
RUN 커맨드 실행
CMD 데몬 실행
LABEL 라벨 지정
EXPOSE 포트 export
ENV 환경변수 설정
ADD 파일 및 디렉터리 추가
COPY 파일 복사
VOLUME 볼륨 마운트
ENTRYPOINT 데몬 실행
USER 사용자 설정
WORKDIR 작업 디렉터리 지정
ONBUILD build 완료 후 실행될 명령어
````
- ex
````
ADD <호스트 파일 경로> <Docker 이미지 파일 경로>
````
- 이미지 생성
````
docker build <옵션> <Dockerfile 경로>
````

### Docker Compose
- 여러 컨테이너 하나로 관리
- docker-compose.yml
- 베이스 이미지 지정
````
image: 이미지 이름
````
````
build: Dockerfile 위치
````

### 링크
http://pyrasis.com/docker.html
https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html