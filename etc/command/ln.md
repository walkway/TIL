# ln

- 리눅스 파일시스템에서 링크파일을 만드는 명령어

### 심볼릭링크 (Symbolic Link)
- 원본파일을 가리키도록 링크만 시켜둔 것
- 원본파일을 가리키고만 있으므로 원본파일의 크기와는 무관하다.

### 하드링크 (Hard Link)
- 원본파일과 다른 이름으로 존재하는 동일한 파일
- 원본파일과 링크파일 두개가 서로 다른 파일이기 때문에 둘 중 하나를 삭제하더라도 나머지 하나는 그대로 남아 있다.
- 원본파일의 내용이 변경될 경우에는 링크파일의 내용이 자동으로 변경된다.

````
ln [옵션] 원본파일 대상파일(대상디렉토리)
````

### 옵션
````
--backup[=CONTROL] : 대상파일이 이미 존재할 경우에 백업파일을 만든 후에 링크파일 생성
-b : 링크파일 생성시에 대상파일이 이미 존재하면 백업파일을 만든 후에 링크파일을 생성
-d : 디렉토리에 대한 하드링크파일생성을 가능하게 함. 단 root 권한으로 수행하더라도 시스템의 권한제한으로 인하여 실패할 가능성이 높음. (-F 와 --directory 는 -d 와 동일함)
-f : 대상파일이 존재할 경우에 대상파일을 지우고 링크파일을 생성
-i : 대상파일이 존재할 경우에 대상파일을 지울것인가를 확인요청 (--interactive 와 동일)
-s : 심볼릭 링크파일을 생성
-S : 백업파일 생성시에 원하는 접미사(suffix)를 지정할 수 있음
-t, --target-directory=DIRECTORY : 링크파일을 생성할 디렉토리를 지정
````