# grep

- 현재 실행 중인 프로세스에서 grep 단어 들어가는 것은 제외
````
ps -ef | grep $process_name | grep -v "grep"
````

- script로 실행하는 경우
````
ps ax | grep $process_name | grep -v "grep" | grep -v $0
````

- 프로세스 ID 추출
````
ps ax | grep $process_name | grep -v "grep" | grep -v $0 | awk "{print $1}"
````