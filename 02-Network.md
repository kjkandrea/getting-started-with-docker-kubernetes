## 도커 네트워크 

도커는 컨테이너에 내부 IP를 순차적으로 할당하며, 이 IP는 컨테이너를 재시작 할 때마다 변경된다.

외부와 통신해야할 경우 사용자의 선택에따라 여러 네트워크 드라이버를 쓸 수 있다.

사용할 수 있는 네트워크 목록을 확인 

```bash
docker network ls
```

### 브리지 네트워크

```bash
docker network inspect bridge
```

브리지 네트워크는 사용자 정의 브리지를 새로 생성하여 각 컨테이너에 연결하는 네트워크 구조이다.

다음 명령어를 이용하여 새로운 브리지 네트워크를 생성한다.

```bash
docker network create --driver bridge mybridge
```

```bash
docker run -i -t --name mynetwork_container \
--net mybridge \
ubuntu:14.04
```

이후 컨테이너 내부에서 ifconfig 로 새로운 IP 대역이 할당된 것을 알 수 있다.

사용자 정의 브리지 네트워크는 connect, disconnect 를 통해 유동적으로 붙이고 뗄 수 있다.

```bash
docker network disconnect mybridge mynetwork_container
docker network connect mybridge mynetwork_container
```

```bash
docker network inspect mybridge
```

```json
[
    {
        "Name": "mybridge",
        "IPAM": {
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Containers": {
            "f846568eea06bac1e9a3823c4a96a013c4aa7df6318ed253540ca1e28a434fef": {
                "Name": "mynetwork_container",
                "EndpointID": "35db146bdb4f6329c4a0ce19e3e8d20208776a3fbabf74ca19211f474e17428e",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        }
    }
]
```

### 호스트 네트워크

컨테이너의 내부의 애플리케이션을 별도의 포트 포워딩 없이 서비스 할 수 있다.

```bash
docker run -it --name network_host \
--net host \
ubunbu:14.04
```
```bash
ifconfig
```

### 컨테이너 네트워크

다른 컨테이너와 네트워크 네임스페이스 환경을 공유할 수 있다.
내부 IP를 새로 할당받지 않으며 호스트에 veth로 시작하는 사강 네트워크 인터페이스도 생성되지 않는다.

```bash
docker run -i -t -d --name network_container_1 ubuntu:14.04

docker run -i -t -d --name network_container_2 \
--net container:network_container_1 \
ubuntu:14.04
```

```bash
docker exec network_container_1 ifconfig

docker exec network_container_2 ifconfig
```

## 컨테이너 로깅

도커는 컨테이너의 표준 출력(StdOut) 과 에러(StdErr) 로그를 별도의 메타데이터 파일로 저장하여 이를 확인하는 명령어를 제공한다.

```bash
docker run -d --name mysql \
-e MYSQL_ROOT_PASSWORD=1234 \
--platform linux/amd64 mysql:5.7
```
```bash
docker logs mysql
```

logs 디버깅을 위해 의도적으로 ROOT PASSWORD 를 누락하고 mysql 컨테이너 생성

```bash
docker run -d --name no_passwd_mysql \
--platform linux/amd64 mysql:5.7
```

```bash
docker logs no_passwd_mysql
```

```bash
2023-12-07 02:16:26+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.44-1.el7 started.
2023-12-07 02:16:27+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2023-12-07 02:16:27+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.44-1.el7 started.
2023-12-07 02:16:28+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
    You need to specify one of the following as an environment variable:
    - MYSQL_ROOT_PASSWORD
    - MYSQL_ALLOW_EMPTY_PASSWORD
    - MYSQL_RANDOM_ROOT_PASSWORD

```