# DockerSwarm

운영 시 확장성 측면에서 여러 대의 서버를 클러스터로 만들어 자원을 병렬로 확장해야할 수 있다.

도커 스웜은 다음의 문제를 해결한다.
* 새로운 서버나 컨테이너가 추가됐을 때 Service Discovery
* 어떤 서버에 컨테이너를 할당할 것인가에 대한 스케줄러와 로드밸런서 문제
* 클러스터 내에 서버가 다운됐을 때 고가용성(High Availability) 를 보장

## 스웜 클래식 vs 도커 스웜 모드

* **스웜 클래식** : 여러 대의 도커 서버를 하나의 지점에서 사용하도록 단일 접근점을 제공
* **스웜 모드** : 마이크로서비스 아키텍처의 컨테이너를 다루기 위한 클러스터링 기능에 초점

**스웜모드** 가 서비스 확장성과 안정성 등 여러 측면에서 스웜 클래식보다 뛰어나기 때문에 일반적으로는 스웜 모드를 더 많이 사용한다.
스웜 모드는 서비스 장애에 대비한 고가용성과 부하 분산을 위한 로드밸런싱 기능을 제공한다.

## 도커 스웜 모드의 구조

* **워커 노드** : 실제로 컨테이너가 생성되고 관리되는 도커 서버 
* **매니저 노드** : 워커 노드를 관리하기 위한 도커 서버. 기본적으로 워커 노드의 역할을 포함한다.
