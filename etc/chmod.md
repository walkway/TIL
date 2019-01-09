# chmod

- change mode
- 유닉스와 유닉스 계통 환경 안에서 쓰이는 셸 명령어
- 파일들이나 디렉터리의 파일 시스템 모드 변경

````
$ chmod [options] mode[,mode] file1 [file2 ...]
````
````
r(4)	읽기 (read)	파일을 읽거나 디렉터리 안 내용물의 리스트를 보여준다
w(2)	쓰기 (write)	파일이나 디렉터리에 쓴다
x(1)	실행하기 (excute)	파일을 실행하거나 디렉터리 트리로 되돌아간다
````

- 파일 타입
````
d 디렉토리
l(소문자 L) 링크 카운터
s 소켓
p 파이프
- 일반
c 특수문자
b 특수블럭
````

- 권한 그룹
````
user : 소유자에 대한 권한
group : 소유자그룹에 대한 권한
others : 기타 사용자에 대한 권한
````

- example
````
chmod +r file			모두에 대해 읽기를 추가한다
chmod -x file			모두에 대해 실행 허가를 제거한다
chmod u=rw,go= file		파일의 읽기와 쓰기를 그 파일의 소유자에게만 설정하고 그룹이나 다른 사람들에게 설정된 모든 허가를 지운다
chmod +rw file			모두가 읽고 쓸 수 있도록 file이라는 파일의 허가들을 바꾼다
chmod 0 file			모두의 모든 허가를 지운다
chmod 666 file			그 사용자, 그 그룹이나 다른 모든 이에 대해서 쓰기나 읽기 권한을 설정한다
chmod 0755 file			u=rwx (4+2+1)나 go=rx (4+1 & 4+1)와 같다. 0는 어떤 특별한 모드들도 지정하지 않는다.
chmod 4755 file			4은 사용자 ID 설정을 지정하고 나머지는 u=rwx (4+2+1)나 go=rx (4+1 & 4+1)와 동일하다
````