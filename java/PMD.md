# PMD

- 자바 정적 분석 도구
- 잠재적 버그, 사용하지 않는 코드, 복잡한 코드, 중복 코드 등 체크

### Rule Categories
- Best Practices: 일반적으로 허용되는 모범 사례를 시행하는 규칙
- Code Style: 특정 코딩 스타일을 적용
- Design: 디자인 문제를 발견하는 데 도움이되는 규칙
- Documentation: 코드 문서와 관련
- Error Prone: 오류가 발생하기 쉬운 구조를 탐지하는 규칙
- Multithreading: 멀티 스레드 실행을 처리 할 때 문제가 있음을 나타내는 규칙
- Performance: 차선의 코드를 나타내는 규칙
- Security: 보안 결함을 표시하는 규칙

````
<?xml version="1.0"?>
<ruleset name="Custom ruleset"
        xmlns="http://pmd.sourceforge.net/ruleset/2.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 http://pmd.sourceforge.net/ruleset_2_0_0.xsd">
    <description>
        This ruleset checks my code for bad stuff
    </description>
</ruleset>
````
````
<?xml version="1.0"?>
<ruleset name="Custom ruleset"
		xmlns="http://pmd.sourceforge.net/ruleset/2.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 http://pmd.sourceforge.net/ruleset_2_0_0.xsd">

	<description>
		This ruleset checks my code for bad stuff
	</description>

	<!-- Here's some rules we'll specify one at a time -->
	<rule ref="category/java/bestpractices.xml/UnusedLocalVariable"/>
	<rule ref="category/java/bestpractices.xml/UnusedPrivateField"/>
	<rule ref="category/java/codestyle.xml/DuplicateImports"/>
	<rule ref="category/java/errorprone.xml/UnnecessaryConversionTemporary"/>

	<!-- We want to customize this rule a bit, change the message and raise the priority  -->
	<rule ref="category/java/errorprone.xml/EmptyCatchBlock"
			message="Must handle exceptions">
		<priority>2</priority>
	</rule>

	<!-- Now we'll customize a rule's property value -->
	<rule ref="category/java/design.xml/CyclomaticComplexity">
		<properties>
			<property name="reportLevel" value="5"/>
		</properties>
	</rule>

	<!-- We want everything from category Code Style except WhileLoopsMustUseBraces -->
	<rule ref="category/java/codestyle.xml">
		<exclude name="WhileLoopsMustUseBraces"/>
	</rule>
</ruleset>
````
````
<?xml version="1.0"?>
<ruleset name="myruleset"
		xmlns="http://pmd.sourceforge.net/ruleset/2.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 http://pmd.sourceforge.net/ruleset_2_0_0.xsd">
	<description>My ruleset</description>
	<exclude-pattern>.*/some/package/.*</exclude-pattern>
	<exclude-pattern>.*/some/other/package/FunkyClassNamePrefix.*</exclude-pattern>
	<include-pattern>.*/some/package/ButNotThisClass.*</include-pattern>
	<rule>...
</ruleset>

````
1. Basic(rulesets/basic.xml)
 - 대부분의 개발자들이 동의하는 규칙
 - catch 블록들은 비어있어서는 안 된다.
 - equals()를 overriding 할 때 마다 hashCode()를 override한다.
2. Naming(rulesets/naming.xml)
 - 표준 자바 naming 규약을 위한 테스트
 - 변수 이름들은 너무 짧아서는 안 된다.
 - 메소드 이름은 너무 길어서는 안 된다.
 - 클래스 이름은 대문자로 시작해야 한다.
 - 메소드와 필드 이름들은 소문자로 시작해야 한다.
3. Unused code(rulesets/unusedcode.xml)
 - 불필요한 코드 제거
 - 결코 읽히지 않은 private field와 local variable
 - 접근할 수 없는 문장, 결코 호출되지 않는 private method 등
4. Design(rulesets/design.xml)
 - 다양하고 좋은 디자인 원리 체크
 - switch 문장은 default 블록을 갖고 있어야 한다.
 - 심하게 중첩된 if 블록은 피해야 한다
5. Import statements (rulesets/imports.xml)
 - import 문장에 대한 문제들 점검
 - 같은 클래스를 두 번 반입하는 것
 - java.lang에서 클래스를 import하는 것
6. JUnit tests(rulesets/junit.xml)
 - Test Case와 Test Method
 - 관련 특정 문제 검색 Method 이름의 정확한 스펠링 등
7. Strings(rulesets/string.xml)
 - 스트링 관련 작업을 할 때 발생하는 일반적인 문제들 규명
8. Braces(rulesets/braces.xml)
 - for, if, while, else 문장이 괄호를 사용하는지 여부 검사
9. Code size(rulesets/codesize.xml)
 - 과도하게 긴 메소드, 너무 많은 메소드를 가진 클래스 검사
10. Javabeans(rulesets/javabeans.xml)
 - 직렬화 될 수 없는 bean 클래스 같이 JavaBeans 코딩 규약을 위배하는 JavaBeans 컴포넌트 검사
11. Finalizers
 - finalize() 메소드 관련한 다양한 문제들을 검사
 - 비어있는 finalizer, 다른 메소드를 호출하는 finalize() 메소드
 - finalize()로의 호출 등
12. Clone(rulesets/clone.xml)
 - clone() 메소드에 대한 규칙
 - clone()을 오버라이드하는 클래스는 Cloneable을 구현해야 한다.
13. Coupling(rulesets/coupling.xml)
 - 클래스들간 과도한 커플링 표시 검색
 - 지나치게 많은 import, 너무 적은 필드, 변수, 클래스 내의 리턴 유형 등
14. Strict exceptions (rulesets/strictexception.xml)
 - 예외 테스트 메소드는 java.lang.Exception을 발생 시키도록 선언되어서는 안 됨
 - 예외는 플로우 제어에 사용되어서는 안 됨
15. Controversial(rulesets/controversial.xml)
 - 좀 더 의심스러운 검사 변수에 null 할당하기
16. Logging (rulesets/logging-java.xml)
 - java.util.logging.Logger를 위험하게 사용하는 경우 검색
 - 끝나지 않고 정적이지 않은 logger와 한 클래스에 한 개 이상의 logger 등

### Suppressing warnings
- Annotation
````
@SuppressWarnings("PMD.UnusedLocalVariable")
````
- 주석, 라인 제거
````
public class Foo {
 void bar() {
  int x = 42;
  if (x > 5) { // NOPMD
  }
 }
}
````
공개SW를 활용한 소프트웨어 개발보안 점검가이드
https://pmd.github.io/pmd-6.1.0/index.html