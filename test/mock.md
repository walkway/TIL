# @injectMocks

- SampleService 클래스에서 SampleDAO를 주입받는 경우
- mock(SampleDAO.class); == @Mock
- @Mock: MockitoJUnitRunner.class가 자동으로 mock 객체 생성
````
@RunWith(MockitoJUnitRunner.class)
public class SampleServiceTest {
	@Mock
	private SampleDAO sampleDAO;

	@InjectMocks
	private SampleService sampleService = new SampleService();
````
