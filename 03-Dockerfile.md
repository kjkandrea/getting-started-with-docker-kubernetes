## Dockerfile

해결하는 문제 : 애플리케이션이 동작하는 환경을 구성하기 위해 일일이 수작업으로 패키지를 설치하고 소스코드를 깃(Git) 에서 복제하거나 호스트에서 복사하는 작업을 자동화한다.

완성된 이미지를 생성하기 위해 컨테이너에 설치해야 하는 패키지, 추가해야 하는 소스코드, 실행해야 하는 명령어와 셸 스크립트 등을 Dockerfile 에 기록해둔다.

생성한 이미지를 DockerHub 등에 배포할 때 이미지 대신 Dockerfile 을 배포할 수 있다.

* 애플리케이션에 필요한 패키지 설치등을 명확히 나타낸다.
* 이미지 생성을 자동화 한다.
* 쉽게 배포할 수 있다.

### Dockerfile 작성

[03-Dockerfile/dockerfile](./03-Dockerfile/dockerfile)

### Dockerfile 빌드

#### 이미지 빌드

```bash
docker build -t mybuild:0.0 ./
```

#### 이미지로 컨테이너를 실행

```bash
docker run -d -P --name myserver mybuild:0.0
```