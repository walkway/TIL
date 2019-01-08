# open jdk

- java 프로그래밍 언어: GPL 라이센스 무료
- java 프로그램 컴파일(jdk) -> 실행(jvm)
- OpenJDK(Open Java Development Kit)는 자바 플랫폼, 스탠더드 에디션 (자바 SE)의 자유-오픈 소스 구현체
- jdk 설치하면 jvm도 함께 설치
- jdk: oracle(상업), open jdk(오픈 소스)
- oracle jdk
 - BCL -> java se subscription 유료 구독형 라이센스 개편
 - 재산권이 있는 플러그인 제공
 - oracle jdk(java se): jdk, jre(jvm), javafx sdk, javafx runtime, jrocki jdk
- open jdk: GNU GPL v2

### mac uninstall java

- 심볼릭 링크, 파일 삭제
````
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 
sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefPane 
sudo rm -fr ~/Library/Application\ Support/Oracle/Java
````
````
cd /Library/Java/JavaVirtualMachines
sudo rm -rf [버전].jdk
````
- 다음의 java 링크는 건들이지 않음
````
usr/bin
````

### open jdk install
- zulu을 사용하여 open jdk 설치
````
brew cask install zulu8
````
- 최신 버전 설치
````
brew cask install zulu
````
- 버전 확인
````
java -version
````
- intellij jdk 설정
````
File > Project Structure > Platform Settiongs > SDKs
````

http://taewan.kim/post/openjdk/
http://jsonobject.tistory.com/395