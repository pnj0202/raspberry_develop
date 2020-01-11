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
     <name>hbase.unsafe.stream.capability.enforce</name>
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

sudo vi /etc/opentsdb/opentsdb.conf 에서
tsd.network.port=4242
tsd.http.staticroot= ./buuild/staticroot
tsd.http.cachedir= /tmp/tsd
```
## 프로그램 실행
```
/usr/local/opentsdb 에서 실행
sudo ./build/tsdb tsd --auto-metric
```
## 접속하는 방법
```
http://192.168.0.12 (ip 주소):4242(포트번호)
```

# Grafana Installation

## 1. Repository의 GPG key를 더하기
```
curl https://bintray.com/user/downloadSubjectPublicKey?username=bintray | sudo apt-key add -
```
* PC의 경우 위 대신 다음을 실행
```
curl https://packagecloud.io/gpg.key | sudo apt-key add -
```

## 2. Repository를 더하기
```
echo "deb https://dl.bintray.com/fg2it/deb stretch main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
* PC의 경우 위 대신 다음을 실행
```
echo "deb https://packagecloud.io/grafana/stable/debian/ stretch main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
echo "deb https://packagecloud.io/grafana/testing/debian/ stretch main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

## 3. 프로그램 설치
```
sudo apt update
sudo apt install grafana
```

## 4. 프로그램 실행
```
sudo service grafana-server start
```

## 5. 참고자료
* 라즈베리파이
  * https://www.circuits.dk/install-grafana-influxdb-raspberry/
* PC
  * http://docs.grafana.org/installation/debian/
  
  ## vnc 접속방법
```
라즈베리파이 먼저 접속
$ vncserver
ip 주소를 보내면
vnc 뷰어에서 그대로 접속 하면 됨(vnc 뷰어는 구글에서 윈도우즈용으로 다운받아 설치 하면 됨)
```

