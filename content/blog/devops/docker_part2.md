---
title: 'docker part-2'
date: '2019-11-24T22:39:21.169Z'
category: 'devops'
---

![](./images/docker-logo.png)

# Docker 파트-2

### 도커이미지를 빌드해서 배포해 보자 feat. AWS

#### 도커파일

> Dockerfile이란 Docker Image를 만들기 위한 설정 파일입니다. 여러가지 명령어를 토대로 Dockerfile을 작성하면 설정된 내용대로 Docker Image를 만들 수 있습니다. 이전 포스트에서도 설명했듯이 Dockerfile을 읽을줄 안다는 것은 해당 이미지가 어떻게 구성되어 있는지 알 수 있다는 의미입니다.

### Docker File 작성 예
```vim
$ vim Dockerfile

# 기반 이미지 
FROM python:3

# 프로젝트 소스가 실행될 위치
WORKDIR /usr/src/app

## Install packages
COPY requirements.txt ./
RUN pip install -r requirements.txt

## Copy all src files
COPY . .

## Run the application on the port 8080
EXPOSE 8000

#CMD ["python", "./setup.py", "runserver", "--host=0.0.0.0", "-p 8080"]
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "order.wsgi:application"]


:wq!
```

- FROM 

  > 기반이 되는 이미지 레이어입니다.

  > <이미지 이름>:<태그> 형식으로 작성 

  > ex) python:3 파이썬 버전 3가 셋팅된 이미지파일

- WORKDIR  

  > CMD에서 설정한 실행 파일이 실행될 디렉터리입니다.

- EXPOSE 

  > 호스트와 연결할 포트 번호입니다.

- COPY

  > 현재 경로의 소스를 생성되는 이미지의 WORKDIR에 복사 .

- CMD

  > 컨테이너가 시작되었을 때 실행할 실행 파일 또는 셸 스크립트입니다. 

  > 해당 명령어는 DockerFile내 1회만 쓸 수 있습니다.

### .dockerignore

  > dockerignore 파일 생성시 Docker 이미지 생성 시 이미지안에 들어가지 않을 파일을 지정 할 수 있습니다.

```vim
$ vim dockerignore

node_modules

npm-debug.log

Dockerfile*

docker-compose*

.dockerignore

.git

.gitignore

README.md

LICENSE

.vscode


:wq!
```


#### 작성 된 Docker File로 Image 만들기

  > docker build -t '도커 계정명/프로젝트명:태그' .

  > ex) docker build -t abcd/order:0.1 .

  > 해당 명령어는 반드시 DockerFile 경로에서 입력해야 한다.

#### 별 문제가 없다면 이미지 빌드가 잘 됐을 것이다.
  > docker images

#### 실행을 해보자
  > docker run --name order -p 8000:8000 -d abcd/order:0.1

  > 위와 같이 실행하면 order라는 이름의 컨테이너가 로컬 8000 포트를 통해 잘 실행될 것이다.

#### 실행중인 도커에 접속하는 방법?
  > docker exec -it order /bin/bash

#### 그렇다면 이제 자신의 저장소에 로그인하고 push한다.
  > docker login 
  
  > docker push abcd/order:0.1

#### 배포를 원하는 aws EC2 서버에 접속해서 Docker 설치 후 로그인 후 풀하고 실행해보자
  > docker pull abcd/order:0.1
  
  > docker run --name order -p 8000:8000 -d abcd/order:0.1

#### 위와 같은 과정을 통하면 수동적인 절차지만 초심자 입장에서 도커파일을 통해 이미지를 빌드하고 배포하는 가장 기본적인
#### 과정을 익혔다고 볼 수 있다. 차후에는 이를 조금 더 자동화하는 과정을 진행해 보도록 하겠다.
#### 오늘은 여기까지!