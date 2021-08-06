# JUnit 5
- JUnit5 = JUnit Platform + JUnit Jupiter + JUnit Vintage
- JUnit Platform : JUnit Platform은 JVM에서 테스트 프레임워크를 시작하기 위한 기초적인 역할을 수행, 테스트 개발을 위한 API를 제공
- JUnit Jupiter : JUnit5에서 테스트 및 Extension을 작성하기 위한 새로운 프로그래밍 모델과 확장 모델의 조합, TestEngine 제공
- JUnit Vintage : 하위 호환성을 위해서 JUnit4, JUnit3를 실행할 수 있는 TestEngine
- java 8 부터 지원 (이전 버전도 컴파일 정상적으로 지원)

## 주요 어노테이션

### @DisplayName
- 테스트 클래스나 테스트 메소드에 이름을 붙여줄 때 사용한다.
- 기본적으로 테스트 이름은 메소드 이름을 사용한다. 메소드 이름은 그대로 둔 채 테스트 이름을 바꾸싶을 때 사용한다.
````
@Test
@DisplayName("이름 수정")
void update_name() {
	
}
````

### @DisplayNameGeneration
- Standard : 메소드이름과 그 뒤에 붙는 괄호 그대로 보여준다.
- Simple : 메소드이름만 보여준다.
- ReplaceUnderscores : 언더스코어를 제거한다. 게시판_테스트 를 게시판 테스트 로 보여준다.
- IndicativeSentences : 테스트 클래스 이름과 테스트 메소드이름 + 괄호를 보여준다.
````
@Test 메소드 이름에 언더바(_)로 표시한 부분은 공백으로 처리한다.
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
class SomeTest {
  ...
}
````

### @BeforeEach
- 각각 테스트 메소드가 실행되기전에 실행되어야 하는 메소드를 명시해준다. @Test, @RepeatedTest, @ParameterizedTest, @TestFactory 가 붙은 테스트 메소드가 실행하기 전에 실행된다.
- JUnit4의 @Before 와 같은 역할을 한다.

### @AfterEach
- @Test, @RepeatedTest, @ParameterizedTest, @TestFactory 가 붙은 테스트 메소드가 실행되고 난 후 실행된다.
- JUnit4의 @After 어노테이션과 같은 역할을 한다.

### @BeforeAll
- 테스트가 시작하기 전 한 번만 실행 된다. (@BeforeEach 는 각 테스트 메소드 마다 실행)
- static 메소드

### @AfterAll
- 테스트가 완전히 끝난 후 한 번만 실행 된다.
- static 메서드

### @Nested
- 여러 테스트 그룹 간의 관계를 표현할 수 있다. non-static인 inner 클래스에만 @Nested를 붙일 수 있다.
- inner 클래스 안에서 static 멤버 변수를 두는 것을 허락하지 않기 때문에 @BeforeAll, @AfterAll 메서드는 동작하지 않는다.
- (@TestInstance(Lifecycle.PER_CLASS)를 붙이면 우회하여 사용할 수 있음)
````
@DisplayName("A stack")
class TestingStack {

    Stack<Object> stack;

    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }

    @Nested
    @DisplayName("when new")
    class WhenNew {
        @BeforeEach
        void createNewStack() {
            stack = new Stack<>();
        }

        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }

        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }

        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {
            String anElement = "an element";

            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }

            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
````

### @Tag
- 테스트를 필터링할 때 사용한다. 클래스, 메소드 레벨에 사용한다.

### @TestMethodOrder
- DisplayName : displayName 기반으로 정렬한다.
- MethodName : 메소드 이름으로 정렬한다.
- OrderAnnotation : @Order 어노테이션에 명시된 순서대로 정렬한다.
- Random : 랜덤으로 정렬한다.

### @Disabled
- 테스트 클래스나, 메소드의 테스트를 비활성화 한다. JUnit4의 @Ignore와 같다.
- 비활성화 하는 이유를 설명하는 것을 권장한다.
````
// 클래스 선언
@Disabled("#1 이슈가 해결될 때 까지 비활성화")
public class DisabledClassTest {

    @Test
    void testWillBeSkipped() {
    }
}

// 메소드 선언
public class DisabledTest {
    @Disabled("#1 버그가 해결될 때 까지 비활성화")
    @Test
    void testWillBeSkipped() {
    }

    @Test
    void testWillBeExecuted() {
    }
}
````

### @Timeout
- 주어진 시간안에 테스트가 끝나지 않으면 실패한다.

#### @ExtendWith
- extension을 등록한다.

### @RegisterExtension
- 필드를 통해 extension을 등록한다. private이 아니라면 상속된다.

### @TempDir
- 필드 주입이나 파라미터 주입을 통해 임시적인 디렉토리를 제공할 때 사용한다.

### @RepeatedTest
- 어노테이션에 명시된 숫자로 테스트를 반복적으로 실행할지 지정할 수 있다.
````
public class RepeatTest {

    @BeforeEach
    void beforeEach(TestInfo testInfo, RepetitionInfo repetitionInfo) {
        int currentRepetition = repetitionInfo.getCurrentRepetition();
        int totalRepetitions = repetitionInfo.getTotalRepetitions();
        String methodName = testInfo.getTestMethod().get().getName();
        log.info(String.format("About to execute repetition %d of %d for %s",
                currentRepetition, totalRepetitions, methodName));
    }

    @RepeatedTest(10)
    void repeatedTest() {
    }
}

Console
INFO: About to execute repetition 1 of 10 for repeatedTest
INFO: About to execute repetition 2 of 10 for repeatedTest
INFO: About to execute repetition 3 of 10 for repeatedTest
INFO: About to execute repetition 4 of 10 for repeatedTest
INFO: About to execute repetition 5 of 10 for repeatedTest
INFO: About to execute repetition 6 of 10 for repeatedTest
INFO: About to execute repetition 7 of 10 for repeatedTest 
INFO: About to execute repetition 8 of 10 for repeatedTest
INFO: About to execute repetition 9 of 10 for repeatedTest 
INFO: About to execute repetition 10 of 10 for repeatedTest
````

### @ParameterizedTest
- 각각 다른 인자로 여러번 테스트를 돌린다.
````
@ParameterizedTest
@ValueSource(strings = {"racecar", "radar", "able was I ere I saw elba"})
void palindromes(String candidate) {
	assertTrue(StringUtils.isPalindrome(candidate));
}

Console
palindromes(String) ✔
├─ [1] candidate=racecar ✔
├─ [2] candidate=radar ✔
└─ [3] candidate=able was I ere I saw elba ✔
````

- 인자 제공
  - @NullSource : @ParameterizedTest 메소드에 null을 제공한다.
  - @EmptySource : 다음과 같은 타입 String ,List ,Set, Map, primitive 배열 예) int[] ,char[] …, 객체 배열 예) String[] ,Intger[] … 같은 인자에 빈값을 제공한다.
  - @EnumSource는 Enum을 편리하게 사용하게 해 준다.
````
@ParameterizedTest
@EnumSource(names = { "DAYS", "HOURS" })
void testWithEnumSourceInclude(ChronoUnit unit) {
  assertTrue(EnumSet.of(ChronoUnit.DAYS, ChronoUnit.HOURS).contains(unit)); 
}
````

## 테스트 클래스, 메소드
- 테스트 클래스: 최상위 클래스, static 멤버 클래스, @Nested 클래스에 적어도 한개의 @Test 어노테이션이 달리 ㄴ테스트 메소드가 포함되어있는 것을 말한다.
- 테스트 메소드: @Test ,@RepeatedTest ,@ParamterizedTest,@TestFactory ,@TestTemplate 같은 메타 어노테이션이 붙여진 메소드다.
- 라이프 사이클 메소드: @BeforeAll , @AfterAll , @BeforeEach , @AfterEach 같은 메타 어노테이션이 붙여진 메소드다.

## Assertions
- JUnit Jupiter는 JUnit4로부터 온 assertion 메소드와 새롭게 자바 8 람다 표현식으로 추가된 메소드들이 있다.
- 모든 JUnit Jupiter assertion은 정적 메소드이며, org.junit.jupiter.api.Assertions 클래스 안에 있다.
````
import static java.time.Duration.ofMillis;
import static java.time.Duration.ofMinutes;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTimeout;
import static org.junit.jupiter.api.Assertions.assertTimeoutPreemptively;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.concurrent.CountDownLatch;

import example.domain.Person;
import example.util.Calculator;

import org.junit.jupiter.api.Test;

class AssertionsDemo {

    private final Calculator calculator = new Calculator();

    private final Person person = new Person("Jane", "Doe");

    @Test
    void standardAssertions() {
        assertEquals(2, calculator.add(1, 1));
        assertEquals(4, calculator.multiply(2, 2),
                "The optional failure message is now the last parameter");
        assertTrue('a' < 'b', () -> "Assertion messages can be lazily evaluated -- "
                + "to avoid constructing complex messages unnecessarily.");
    }

    @Test
    void groupedAssertions() {
        // In a grouped assertion all assertions are executed, and all
        // failures will be reported together.
        assertAll("person",
            () -> assertEquals("Jane", person.getFirstName()),
            () -> assertEquals("Doe", person.getLastName())
        );
    }

    @Test
    void dependentAssertions() {
        // Within a code block, if an assertion fails the
        // subsequent code in the same block will be skipped.
        assertAll("properties",
            () -> {
                String firstName = person.getFirstName();
                assertNotNull(firstName);

                // Executed only if the previous assertion is valid.
                assertAll("first name",
                    () -> assertTrue(firstName.startsWith("J")),
                    () -> assertTrue(firstName.endsWith("e"))
                );
            },
            () -> {
                // Grouped assertion, so processed independently
                // of results of first name assertions.
                String lastName = person.getLastName();
                assertNotNull(lastName);

                // Executed only if the previous assertion is valid.
                assertAll("last name",
                    () -> assertTrue(lastName.startsWith("D")),
                    () -> assertTrue(lastName.endsWith("e"))
                );
            }
        );
    }

    @Test
    void exceptionTesting() {
        Exception exception = assertThrows(ArithmeticException.class, () ->
            calculator.divide(1, 0));
        assertEquals("/ by zero", exception.getMessage());
    }

    @Test
    void timeoutNotExceeded() {
        // The following assertion succeeds.
        assertTimeout(ofMinutes(2), () -> {
            // Perform task that takes less than 2 minutes.
        });
    }

    @Test
    void timeoutNotExceededWithResult() {
        // The following assertion succeeds, and returns the supplied object.
        String actualResult = assertTimeout(ofMinutes(2), () -> {
            return "a result";
        });
        assertEquals("a result", actualResult);
    }

    @Test
    void timeoutNotExceededWithMethod() {
        // The following assertion invokes a method reference and returns an object.
        String actualGreeting = assertTimeout(ofMinutes(2), AssertionsDemo::greeting);
        assertEquals("Hello, World!", actualGreeting);
    }

    @Test
    void timeoutExceeded() {
        // The following assertion fails with an error message similar to:
        // execution exceeded timeout of 10 ms by 91 ms
        assertTimeout(ofMillis(10), () -> {
            // Simulate task that takes more than 10 ms.
            Thread.sleep(100);
        });
    }

    @Test
    void timeoutExceededWithPreemptiveTermination() {
        // The following assertion fails with an error message similar to:
        // execution timed out after 10 ms
        assertTimeoutPreemptively(ofMillis(10), () -> {
            // Simulate task that takes more than 10 ms.
            new CountDownLatch(1).await();
        });
    }

    private static String greeting() {
        return "Hello, World!";
    }

}
````

## 추가적인 Assertion 라이브러리
- JUnit Jupiter가 제공해주는 assertion이 부족한 경우, 다른 라이브러리를 추가로 사용하는 것을 추천한다.
- Hamcrest
````
import static org.hamcrest.CoreMatchers.equalTo;
import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.MatcherAssert.assertThat;

public class HamcrestAssertionsTest {

    private final Calculator calculator = new Calculator();

    @Test
    void HamcrestMatcher_이용하기() {
        assertThat(calculator.subtract(4, 1), is(equalTo(3)));
    }
}
````

## Assumptions
- 특정 조건이 충족되는 경우에만 테스트를 실행하는 데 사용된다.
- 일반적으로 테스트가 제대로 실행되기 위해 필요한 외부 조건에 사용된다. 테스트와 직접적인 관련은 없다.
- JUnit Jupiter는 JUnit4에서 제공해주던 assumption 메소드와, Java 8 람다 표현식과, 메서드 레퍼런스를 이용해 몇 개를 더 추가했다.
- org.junit.jupiter.api.Assumptions 클래스안에 있다.
````
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;
import static org.junit.jupiter.api.Assumptions.assumingThat;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class AssumptionsDemo {

    private final Calculator calculator = new Calculator();

    @Test
    void testOnlyOnCiServer() {
        assumeTrue("CI".equals(System.getenv("ENV")));
        // remainder of test
    }

    @Test
    void testOnlyOnDeveloperWorkstation() {
        assumeTrue("DEV".equals(System.getenv("ENV")),
            () -> "Aborting test: not on developer workstation");
        // remainder of test
    }

    @Test
    void testInAllEnvironments() {
        assumingThat("CI".equals(System.getenv("ENV")),
            () -> {
                // perform these assertions only on the CI server
                assertEquals(2, calculator.divide(4, 2));
            });

        // perform these assertions in all environments
        assertEquals(42, calculator.multiply(6, 7));
    }

}
````

### 테스트 실행 조건
- JUnit Juptier에 있는 ExecutionCondition API는 개발자가 특정 조건에 따라 테스트를 실행할지 말지 결정한다.
- 개발자가 선언적으로 테스트를 활성화하거나 비활성화 하기 위해 org.junit.jupiter.api.condition 패키지에 있는 어노테이션 기반 조건을 지원한다.
- 여러 개의 ExecutionCondition 이 등록되면 여러 개 조건중 하나라도 비활성화 조건에 걸리면, 테스트를 비활성화 한다.

## 태그와 필터링
- 클래스와 메소드는 @Tag 어노테이션을 통해 태그할 수 있다. 태그는 테스트를 필터링 할때 사용 된다.
- 태그 규칙
  - 태그는 공백이나 null 이 있으면 안됨
  - ISO 제어문자가 있으면 안됨
  - 다음과 같은 문자열이 있으면 안됨 , ( ) | ! &
````
@Tag("fast")
@Tag("model")
class TaggingTest {

    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }
}
````

https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions