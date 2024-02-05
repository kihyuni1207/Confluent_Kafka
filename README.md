# Confluent_Kafka
Kafka 스터디

Confluent Kafka, KsqlDB 사용방법과 실습 과정입니다.

수정해야할 사항있으면 문의 주세요 즉시 수정하도록 하겠습니다.





1. Confluent Kafka 란?

Confluent Cloud의 Apache Kafka는 Apache Kafka를 서비스로 제공하는 Azure Marketplace 제품입니다. 완전 관리형으로, 사용자가 클러스터를 관리하는 대신 애플리케이션 빌드에 집중할 수 있습니다.

플랫폼 간 관리의 부담을 줄이기 위해 Microsoft는 Confluent Cloud와 협력하여 Azure에서 Confluent Cloud까지 통합된 프로비저닝 계층을 빌드했습니다. Azure에서 Confluent Cloud를 사용하기 위한 통합 환경을 제공합니다. Azure 애플리케이션을 사용하여 Confluent Cloud를 쉽게 통합하고 관리할 수 있습니다.

이전에는 Marketplace에서 Confluent Cloud 제품을 구매하고, 별도로 Confluent Cloud에서 계정을 설정해야 했습니다. 구성 및 리소스를 관리하려면 Azure와 Confluent Cloud의 포털 간에 이동해야 했습니다.

이제 Microsoft Confluent라는 리소스 공급자를 통해 Confluent Cloud 리소스를 프로비저닝합니다. Azure Portal, Azure CLI또는 Azure SDK를 통해 Confluent Cloud 조직 리소스를 만들고 관리합니다. Confluent Cloud는 환경, 클러스터, 토픽, API 키, 관리형 커넥터를 포함하여 SaaS(Software as a Service) 애플리케이션을 소유하고 실행합니다.

2. Confluent Kafka 제품 소개

2.1 클라우드 네이티브

[운영 부담을 제거]

오픈 소스 배포, ZooKeeper관리, 파티션 균형조정, 장애 조치및 확장 프로세스 설계 등에 대한 부담을 줄여 단시간안에 배포,운영 및 확장할 수 있는 서비스를 제공한다.

2.2 완전성

[데이터 스트리밍 플랫폼 가치 실현 단축 및 TCO 절감]

비용이 많이 드는 개발 주기를 도구 구축 및 유지 관리에 소비할 필요가 없도록 설계된 종합적인 엔터프라이즈 기능 세트를 제공한다.
* TCO => 총소유비용(하나의 자산을 얻고자 할 때 모든 연관 비용을 고려하는 평가비용)

2.3 Cloud

[하이브리드 및 멀티클라우드 아키텍처]

Kubernetes 어디에서든 모든 주요 퍼블릭 클라우드의 완전 관리형 서비스와 온프레미스 워크로드용으로 배포할 수 있는 자체 관리형 소프트웨어를 자유롭게 활용할 수 있다.

3. Confluent Kafka 설치

3.1 설치환경

GCP

디스크 크기 200(GB)

Ubuntu

3.2 설치방법

https://www.confluent.io/download 접속

SOFTWARE 버튼 클릭후 이름,이메일,회사명 입력후 START FREE 클릭





우측 목록에 보이는 Previous Versions 클릭 후 구버전 목록 확인





Version : 7.1.0 사용 / Installation : Download Community Tarball 설치 





설치 후 압축 해제 (압축 해제 하면 confluent 라는 디렉토리가 생성되어야 합니다.)



GCP 환경에서 테스트 하시는 분들은 로컬에서 설치 후 .tar 파일 자체를 GCP로 옮겨서 테스트 하시면 됩니다.

3.3 설치 후 환경설정

해당 경로로 이동

$confluent/etc/kafka

명령어 실행

$vi server.properties

advertised.listeners 부분 ip 변경 (GCP 외부 ip 입력했습니다.)



4. Confluent Kafka 및 KsqlDB 활용법

kafka를 구동하기 위해서는 zookeeper가 먼저 구동되어야 합니다.

ksqlDB 구동을 위해 docker 설치를 권장합니다.(https://mungiyo.tistory.com/11)

4.1 zookeeper 구동방법

$confluent/bin/zookeeper-server-start confluent/etc/kafka/zookeeper.properties

4.2 kafka 구동방법

$confluent/bin/kafka-server-start confluent/etc/kafka/server.properties

4.3 KsqlDB 구동방법

$confluent/bin/ksql-server-start confluent/etc/ksqldb/ksql-server.properties

4.4 KsqlDB 입력 폼 구동방법

$docker run --net=host --interactive --tty \
   confluentinc/cp-ksql-cli:5.3.1 \
   http://localhost:8088

4.3에서 명령어 실행 후 해당 localhost에 port 번호가 나옵니다.

4.4에 해당되는 명령어 실행시 오류가 난다면 4.3 실행시 나오는 port 번호와 동일하게 4.4 명령어에 입력해 주셔야 합니다.

4.3 실행 후Server 7.1.2 listening on http://0.0.0.0:8089 구동 후 명령어가 보이면

 4.4 명령어에 localhost 부분을 수정해 주세요docker run --net=host --interactive --tty \    confluentinc/cp-ksql-cli:5.3.1 \    http://localhost:8089

4.5 KsqlDB 입력 폼 구동 오류



4.6 KsqlDB 입력 폼 구동 정상



5. Pub/Sub 구조 실습

5.1 개발환경

Intellij

Java 11

Spring Boot

Gradle

5.2 환경설정

java client 개발 진행을 위해 build.gradle안에 3가지 dependencies 추가

implementation 'org.apache.kafka:kafka-clients:3.1.0'

testImplementation 'org.slf4j:slf4j-simple:1.7.36'

implementation 'org.slf4j:slf4j-simple:1.7.25'

KafkaProj-01 이라는 프로젝트 생성 후 멀티 모듈화 생성

KafkaProj-01 프로젝트 하위에 practice, producer 모듈 생성





생성이 완료된 프로젝트 구조도



5.3 개발진행

kafka로 보낼 metaData 생성

데이터 생성이 귀찮으신 분들은 하단에 있는 txt파일을 다운로드 하시고





해당 경로에 txt 파일을 추가해 주세요



마지막으로 해당 경로에 아래 파일을



해당 경로에 추가해 주세요





클래스를 실행시키기 전 kafka 서버에 topic을 생성해 줍니다

Topic 이름은 PizzaProducerCustomPartitioner안에 들어 있는String topicName = "pizza-topic-partitioner";

이름을 그대로 사용하여 만듭니다



Topic 생성 명령어

$confluent/bin/kafka-topics --bootstrap-server localhost:9092 --create --topic pizza-topic-partitioner --partitions 3



해당 클래스를 실행 시켜줍니다



실행시 해당 오류가 발생한다면 Confluent Kafka가 설치된 서버에서 zookeeper와 kafka가 실행되고 있는지 확인해 주셔야 합니다.



정상구동 상태 (메타 데이터가 kafka topic으로 전달되는 중입니다.)



추 후에 GCP서버에 구축된 Kafka에서 어떤식으로 Producer/Consumer하는지 추가로 올릴 예정입니다.
