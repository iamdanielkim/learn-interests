# Tutorial

## 설치 on OSX

* OSX에서는 boot2docker를 통해 docker 환경을 구성한다.
    * 설치 링크 -  https://github.com/boot2docker/osx-installer/releases

## DockerHub 가입

dockerhub에 가입하면 dockerhub에 직접 생성한 image를 등록하고 사용할 수 있다. docker 사이트 또는 ```docker login``` 명령을 통해 가입 또는 로그인을 할 수 있다.


### ```docker login```을 통해 로그인

```
$> docker login
Username: iamdanielkim
Password:
Email: iamdanielkim@gmail.com
Login Succeeded

```

## Dockerizing

### ```docker run``` 으로 App 실행

```docker run```을 통해 특정 image기반의 container에서 명령을 실행한다.

```
$ docker run ubuntu:14.04 /bin/echo 'Hello world'
```

위의 예제는 ubuntu:14.04 Image를 Docker 저장소에서 pull한 후, 주어진 명령을 실행한다.

### Interactive 모드

docker container의 shell 접속을 하고 싶을 경우 아래와 같이 수행하면, shell이 실행된 후 바로 빠져나오게 된다.
```
$ sudo docker run ubuntu:14.04 /bin/bash
```

shell에서 작업을 수행하고자 하면 아래와 같이 ```-t -i``` flag를 같이 추가해야한다.

* ```-i``` 는 stdin에서의 입력을 container에 전달 할 수 있도록 허용하는 flag
* ```-t``` 는 assigns a pseudo-tty or terminal inside our new container

```
$ docker run -t -i ubuntu:14.04 /bin/bash
root@ea0560898737:/# exit 또는 ^D # 실행 종료 & 터미널 나오기

```

### Daemon 모드

```-d``` 를 통해 데몬 형태로 App을 실행할 수 있다.

```
$ docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
90625c558a5659f4d9f32680c7f....

# logs - container의 console 내용 표시
$ docker logs 90625c558a
hello world
hello world
hello world
hello world
hello world
...
# stop - container 중지
$ docker stop  90625c558a
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
# start - 중지된 container 실행
$ docker start  90625c558a
$ docker logs 90625c558a
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
90625c558a56        ubuntu:14.04        "/bin/sh -c 'while t   2 minutes ago       Up 54 seconds                           dreamy_torvalds
```













