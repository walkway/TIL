# Logstash
- 수집 서버에 filebeat 설치, 실행
- logstash input beat 실행, output 설정

````
./bin/logstash-plugin update logstash-input-beats
````
- conf 파일 생성 (bin/logstash.conf)
````
input { 
  beats {
    port => 5044
  }
}

output {
    stdout{
        codec => rubydebug
    }
}
````

- 실행
````
./bin/logstash -f ./logstash.conf
````

# Filebeat
- 권한 설정
````
chmod 644 filebeat.yml
````

- filebeat.yml
````
filebeat.prospectors:
    - input_type: log
        paths:
            - test.log
output.logstash:
    hosts: [ "localhost:5044" ]
````
- 실행
````
./filebeat -e -c filebeat.yml -d "publish"
````

