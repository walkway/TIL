# git

- git pull
	- 서버에서 최신 버전의 workspace 가져온다.
	- workspace 안에서 실행. push안한 사항이 있을 경우 충돌이 발생함. fetch + merge 을 합친 명령어
	- ex) git pull origin master : 해당 branch의 원격 서버에 저장된 최신 내용을 가져온다.
- git reset <파일명>
	- 실수로 잘못 add한 파일 취소
	- add는 했는데 commit하기 전 상황에서 사용
	- stage에서 해당 파일을 삭제한다.
	- git에 의해 파일이 관리를 받지 않도록 한다. (파일명 앞에 HEAD는 생략 가능)
- git rm 파일명
	- commit 된 파일이나 폴더를 삭제한다.
	- 로컬에 있는 것까지 삭제한다.
	- reset만하면 staging만 안하는데, rm은 로컬 삭제까지
- commit 버전 돌아가기
	- git checkout HEAD~5 : 최신 버전에서 5번 뒤로 가기
	- git checkout 9d51a7 : 9d51a7이름의 commit으로 돌아가기
	- git checkout master : 다시 최신버전으로 돌아오기
	- -f 옵션을 추가하면, 수정하던 것을 버리고 해당 버전으로 옮긴다.

http://newsight.tistory.com/25