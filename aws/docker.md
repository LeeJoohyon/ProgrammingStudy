# Docker

## 이미지 삭제

**Delete all docker containers**  
`docker rm $(docker ps -a -q)`

**Delete all docker images**  
`docker rmi $(docker images -q)`

**Delete <none>name docker images force**  
`docker rmi -f $(docker images --filter "dangling=true" -q)`


## 실행중인 컨테이너에 명령 실행

`docker exec [container id] [실행할 명령]`

**접속 후 셸 실행하고 싶을 경우**  
`docker exec -it [container id] /bin/bash`

##build
sudo docker build -t eb .  
					     (프로젝트명)

#가상환경설정 순서
	sudo docker images : 가상환경 보기
	sudo docker pull ubuntu:16.04 : 가상환경 우분투 16.04 설치
	'apt-get update' : 가상환경 우분투에는 정말로 ubuntu만 깔려있기때문에 파이썬 pip 을 설치해야한다
	애초에 root 계정도 하나이기 때문에  sudo가 필요없다.
	apt-get install python3
	apt-get install python3-pip
	apt-get install nginx
	'기본환경 설정'
	
	cd srv
	mkdir app
	cd app
	pip3 install django
	pip3 isntall uwsgi
	
	django-admin startproject eb
	
	이순서로 dockerfile 작성
	
	sudo docker build -t eb-base . -f Dokerfile_base  
	
	docker run -p 4567:8080 eb
			(사용할려는 포트)		(docker 내부포트)

	
