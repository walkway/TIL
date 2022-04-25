# git

### git pull
- 서버에서 최신 버전의 workspace 가져온다.
- workspace 안에서 실행. push안한 사항이 있을 경우 충돌이 발생함. fetch + merge 을 합친 명령어
- ex) git pull origin master : 해당 branch의 원격 서버에 저장된 최신 내용을 가져온다.
- how to avoid merge commit
- rebase option
- 로컬에서 새로운 commit 생성
- pull rebase
````
git pull --rebase <remote-name> <branch-name>
````

### git reset <파일명>
- 실수로 잘못 add한 파일 취소
- add는 했는데 commit하기 전 상황에서 사용
- stage에서 해당 파일을 삭제한다.
- git에 의해 파일이 관리를 받지 않도록 한다. (파일명 앞에 HEAD는 생략 가능)
````
git reset --soft HEAD~1 : 최근 HEAD에서 한단계로 돌아가, commit 한 작업만 취소
git reset --mixed HEAD~1 : 최근 HEAD에서 한단계로 돌아가, commit 한 작업과 add한 작업 취소
git reset --hard HEAD~1 : 최근 HEAD에서 한단계로 돌아가, commit 한 작업과 add한 작업을 삭제(되돌릴 수 없다.)
````
- push한 작업 되돌리기
````
git reset --hard HEAD^
git push --force origin master
````

### git rm 파일명
- commit 된 파일이나 폴더를 삭제한다.
- 로컬에 있는 것까지 삭제한다.
- reset만하면 staging만 안하는데, rm은 로컬 삭제까지

### commit 버전 돌아가기
- git checkout HEAD~5 : 최신 버전에서 5번 뒤로 가기
- git checkout 9d51a7 : 9d51a7이름의 commit으로 돌아가기
- git checkout master : 다시 최신버전으로 돌아오기
- -f 옵션을 추가하면, 수정하던 것을 버리고 해당 버전으로 옮긴다.

### remote
- git push origin :branch명 (branch명 삭제)

### log
````
git log --decorate --all --oneline --graph
````

### commit 
- 현재 commit에서 N개의 commit에 대해 commit 합침, 제거, 메세지 수정 등
- N개의 commit에 대한 해시 값과 메세지를 보여지고, 앞에 pick, squash 등의 명령을 이용할 수 있음
````
git rebase -i HEAD~N
````
vi 편집
````
pick -> s

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
````
````
git push -f origin master
````

http://newsight.tistory.com/25