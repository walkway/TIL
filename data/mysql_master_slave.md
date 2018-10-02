# MySQL 이중화 Master/Slave

- replication: 물리적으로 다른 서버의 저장 공간에 동일한 데이터 복사
- master: create, update, delte
- slave: read
- slave mysql 버전은 master mysql 버전보다 높아야 함.

### 설정 파일
- log-bin: query를 바이너리 파일 로그로 남김
- server-id: replication group 식별 값
- master 설정
````
# ~/mysql/master/my.cnf
[mysqld]
log-bin=mysql-bin  
server-id=1  
````
- slave가 사용할 User 생성, 권한 부여
````
mysql > create user 'usr_repl'@'%' identified by '비밀번호';
mysql > grant file on *.* to 'usr_repl'@'%';
mysql > grant replication slave on *.* to 'usr_repl'@'%' identified by '비밀번호';
mysql > flush privileges;
````
- service mysql restart
- 확인
````
mysql > show master status;
````
- slave 설정
````
# ~/mysql/slave/my.cnf

[mysqld]
server-id=2  
````
````
mysql > stop slave;
mysql > change master to
       -> master_host = 'master ip',
       -> master_port = '3306',
       -> master_user = 'usr_repl',
       -> master_password = '비밀번호',
       -> master_log_file = 'mysql-bin.000001',
       -> master_log_pos = 120;
mysql > start slave;
mysql > show slave status \G;
````