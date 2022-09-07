실행
docker-compose -f docker-compose.yml up -d

종료
docker-compose down

or

docker-compose stop

docker!!
각 컨테이너 log 보기
각 설정값에 image 값을 logs 뒤에 넣어주면 된다.
zookeeper
docker container logs local-zookeeper

kafka
docker container logs local-kafka

컨테이너 접속해보기
docker exec -i -t local-kafka bash

컨테이너 내부 topic log 보기
cd ./kafka/kafka-logs-/test_topic-

Kafka 로 접근해보기!!
공식사이트에서 위 docker-kafka 버전과 같은 kafka를 다운 받는다.
스칼라 : 2.12, 카프카 : 2.3.0
https://kafka.apache.org/downloads
설치
wget http://mirror.navercorp.com/apache/kafka/2.3.0/kafka_2.12-2.3.0.tgz
tar xzvf kafka_2.12-2.3.0.tgz
cd kafka_2.12-2.3.0
topic
옵션
--zookeeper : zookeeper가 실행 중인 호스트
--list : 리스트
--create : topic 생성
--topic : topic 명
--partitions : topic 파티션 수
--replication-factor : topic 복사본 수
생성
bin/kafka-topics.sh –create –zookeeper localhost:2181 –replication-factor 1 –partitions 1 –topic tmp_topic

위 명령어를 실행하면 topic 명 때문에 warning 이 발생한다.
WARNING: Due to limitations in metric names, topics with a period (‘.’) or underscore (‘_’) could collide. To avoid issues it is best to use either, but not both.

tmp_topic 을 tmp-topic 으로 바꿔서 만들자!
확인
bin/kafka-topics.sh –zookeeper localhost:2181 –list

consumer
bin/kafka-console-consumer.sh –bootstrap-server localhost:9092 –topic tmp-topic –from-beginning

위 명령어를 입력하고 새창을 하나 더 띄우자!
producer
bin/kafka-console-producer.sh –broker-list localhost:9092 –topic tmp-topic

무언가를 producer에서 입력하게 되면 consumer에 나오게 된다.
추가
컨테이너 정지 및 제거
docker-compose -f docker-compose.yml stop && docker-compose -f docker-compose.yml rm -vf

참고링크
https://github.com/Yelp/docker-compose
http://www.kwangsiklee.com/2017/03/docker-compose%EB%A1%9C-kafka%EB%A5%BC-%EB%A1%9C%EC%BB%AC%EC%97%90-%EB%9D%84%EC%9B%8C%EB%B3%B4%EC%9E%90/
https://blog.naver.com/PostView.nhn?blogId=occidere&logNo=221390946271&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView
