## [JMXTERM 이용한 Apache Kafka Monitoring 방법]

<br>

### [Usage & Advantage]

- 쉬운 설치(OS 종속 없음)
- 보안 환경에서 활용 가능 (jconsole은 jmx port를 오픈해야 하지만 JMXTerm은 terminal에서 바로 조회 가능)
- 다양한 Broker 접속 (JMX Monitoring 대상 IP:Port를 통해 접속 가능)

<br>

### [구성도]

![image](https://user-images.githubusercontent.com/30817824/172539300-56999f96-8b03-4ea8-8d79-7e8fbea1791c.png)

(refer: https://github.com/freepsw/kafka-metrics-monitoring)

<br><br>

--------------------------------------------

## STG.01 JMXTERM 설치

<br>

#### Java 설치 및 JAVA_HOME 설정
```
> sudo yum install -y java

# 현재 OS 설정이 한글인지 영어인지 확인한다. 
> alternatives --display java

# 아래와 같이 출력되면 한글임. 
슬레이브 unpack200.1.gz: /usr/share/man/man1/unpack200-java-1.8.0-openjdk-1.8.0.312.b07-1.el7_9.x86_64.1.gz
현재 '최고' 버전은 /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.el7_9.x86_64/jre/bin/java입니다.

### 한글인 경우 
> alternatives --display java | grep '현재 /'| sed "s/현재 //" | sed 's|/bin/java로 링크되어 있습니다||'
> export JAVA_HOME=$(alternatives --display java | grep '현재 /'| sed "s/현재 //" | sed 's|/bin/java로 링크되어 있습니다||')

### 영문인 경우
> alternatives --display java | grep current | sed 's/link currently points to //' | sed 's|/bin/java||' | sed 's/^ //g'
> export JAVA_HOME=$(alternatives --display java | grep current | sed 's/link currently points to //' | sed 's|/bin/java||' | sed 's/^ //g')

# 제대로 java 경로가 설정되었는지 확인
# 아래 명령어를 실행하면, "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.el7_9.x86_64/jre"와 유사한 경로가 보여야 함. 
> echo $JAVA_HOME

> echo "export JAVA_HOME=$JAVA_HOME" >> ~/.bash_profile
> source ~/.bash_profile
```

#### Download the jmxterm
```
> cd ~
> curl -OL https://github.com/jiaqi/jmxterm/releases/download/v1.0.2/jmxterm-1.0.2-uber.jar
```

## STG.02 JMXTERM 사용

<br>

```
## jmxterm 실행
> java -jar jmxterm-1.0.2-uber.jar
Welcome to JMX terminal. Type "help" for available commands.

## broker jmx port에 접속
$>open localhost:9999

## 조회 가능한 mbeans domain(metric 그룹)을 확인한다. 
$>domains

## 위 domain 중에 조회할 domain을 선택한다. 
$>domain kafka.server

## "kafka.server" domain에서 제공하는 beans 목록을 확인한다. 
$>beans

## bean 중에서 "MessageInPerSec" metric을 선택한다. 
$>bean kafka.server:name=MessagesInPerSec,type=BrokerTopicMetrics

## "MessageInPerSec"에서 조회 가능한 값의 유형을 선택한다.
$>info

## info에서 출력된 목록 중 원하는 값의 유형을 선택한다. 
$>get Count

$>get MeanRate

## JMXTERM 종료
$>bye
#bye
```
