# git rebase

- git에서 브랜치를 합치는 방법: merge, rebase
- rebase: base를 바꾼다.
````
		c3(develop)
		/
c0-c1-c2-c4(master)
````
- 현재 base c2
- c3에서 변경된 사항을 patch로 만들고, c4에 적용
````
git checkout develop
git rebase master
`````
- 브랜치가 나뉘기 전 공통 commit인 c2로 이동한다.
- c2부터 checkout한 브랜치(develop)가 가리키는 commit까지 diff(c3)를 차례로 만들어 임시로 저장해둔다.(patch)
- rebase할 브랜치(develop)가 합칠 브랜치(master)가 가리키는 commit(c4)을 가리키게 한다.
- 임시 저장해둔 변경사항(patch)을 차례로 적용한다.
````
c0-c1-c2-c4(master)-c3(develop)
````
- master브랜치를 fast-forward한다.
````
c0-c1-c2-c4-c3(master, develop)
````

출처 -https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase%ED%95%98%EA%B8%B0