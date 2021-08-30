# git rebase

- git에서 브랜치를 합치는 방법: merge, rebase
- rebase: base를 바꾼다.
````
	c3(develop)
	/
c0-c1-c2-c4(master)
````
- c2에서 -> 새로운 브랜치 develop
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

## Merge, Squash and Merge, Rebase Merge

### Merge
- 하나의 브랜치와 다른 브랜치의 모든 변경 이력을 합치는 방식으로 Merge Commit에서부터 뒤로 돌아가면서 부모를 모두 찾고 브랜치를 구성한다.
- commit m은 부모로 c, Init을 가지고 있으며, c는 b, b는 a, a는 Init을 다시 부모로 가진다. 이 형상을 모두 backtrace 하여, Init -> a -> b -> c -> m이라는 구조를 만들고 이 구조가 모두 히스토리에 남는다.
````
	a -> b -> c
	/          \
init------------> m 
````

### Squash and Merge
- 각각 commit 되었던 a, b, c를 합쳐서 하나의 새로운 commit을 생성하고 브랜치에 추가한다. 새롭게 생성된 a, b, c 통합 commit은 부모를 init 하나만 가진다.
- commit 'a,b,c' 는 Init만을 부모로 가진 단일 commit이다. 작업했던 브렌치의 a, b, c commit들은 머지 후의 메인 브렌치 커밋 Init, 'a,b,c' 와 아무런 연관을 가지지 않는다.
````
	(a -> b -> c) no
	/          \
init------------> a, b, c 
````

### Rebase and Merge
- 이전에 수행되었던 commit a, b, c 간의 관계를 그대로 유지하면서 메인 브랜치에 추가한다. commit a는 메인 브랜치의 이전 커밋인 commit e를 부모로 가지게 되고, rebase and merge 작업 이후에는 이전에 존재했던 브랜치의 a, b, c commit과 메인 브랜치의 init, d, e, a, b, c commit과 연관을 가지지 않게 된다.
````
	(a -> b -> c) no
	/          
init -> d -> e -> a -> b -> c
````
- https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase%ED%95%98%EA%B8%B0
- https://meetup.toast.com/posts/122