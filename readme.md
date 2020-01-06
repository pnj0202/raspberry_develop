# 라즈베리파이 OpenTSDB설치 방법

## 1.0 libjffi-1.2.so 파일 /usr/lib 이동
## 1.1 프로그램 다운로드
```
cd /usr/local
sudo wget https://archive.apache.org/dist/hbase/2.1.1/hbase-2.1.1-bin.tar.gz
sudo tar xzvf hbase-2.1.1-bin.tar.gz
cd hbase-2.1.1
```

## 1.2 환경설정

`vi conf/hbase-env.sh`에서 `export JAVA_HOME=/usr` 수정

`vi conf/hbase-site.xml`에서 아래 내용처럼 내용을 추가

```xml
<configuration>
   <property>
     <name>hbase.rootdir</name>
     <value>file:///var/local/hbase-2.1.1/hbase</value>
   </property>
   <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/var/local/hbase-2.1.1/zookeeper</value>
   </property>
   <property>
     <name>hbase.unsafe.stream.capability.enforce<name>
     <value>false</value>
     <description>
       Controls whether HBase will check for stream capabilities  (hflush/hsync).
     </description>
     </property>
 </configuration>
 ```
 ## 1.3프로그램시작
 
 `sudo ./bin/start-hbase.sh`
 
 ## 1.4 Java 설치 (설치가안되어있는경우)
 
 
 
 `$ sudo apt-get install openjdk-8-jdk openjdk-8-jre`
 
 # 2.OpenTSDB
 
 ## 2.1필요한 패키지 설치
``` 
 sudo apt update
 sudo apt install gnuplot autotools-dev autoconf make
```
 ## 2.2 프로그램 다운로드 및 빌드
```
 cd /usr/local
 sudo git clone git://github.com/OpenTSDB/opentsdb.git
 cd opentsdb
 sudo ./build.sh
```
## 2.3 HBase에 OpenTSDB 필수 테이블 생성
```
env COMPRESSION=NONE HBASE_HOME=/usr/local/hbase-2.1.1 ./src/create_table.sh
```
## 2.4 환경설정
```
cd /etc
sudo mkdir opentsdb
cd /usr/local/openstsdb
sudo cp src/opentsdb.conf /etc/opentsdb

sudo vi /etc/opentsdb/opentsdb.conf 엥서
tsd.network.port=4242
tsd.http.staticroot= ./buuild/staticroot
tsd.http.cachedir= /tmp/tsd
```
## 프로그램 실행
```
sudo ./build/tsdb tsd --auto-metric
```

