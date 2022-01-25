# Google Cloud Storage

- Object Repository(객체 저장소)
- 데이터의 양에 관계 없이 언제, 어디서나 데이터를 저장하고 가져올 수 있는 서비스
- Project: 데이터가 속하는 공간
- Bucket: 데이터를 담는 개념
  - 버킷안에는 버킷을 만들 수 없다.
  - 이름이 GCP 내에서 Globally unique(전역 상에서 고유)해야 한다.
  - 버킷수는 작고 객체(Object)가 많을수록 좋은 설계이다.
  - 버킷 생성시, 버킷의 '이름', '지리적 위치', 'Repository 등급'을 지정할 수 있다.
  - key-value 형태로 GCP의 다른 리소스와 그룹화가 가능하다.
- Object: 버킷에 저장되는 파일. 객체는 생성 갯수에는 제한이 없음
  - 객체 데이터: GCS(Google Cloud Storage)에 저장되는 파일
  - 객체 메타(meta)데이터 : key-value 값 형태이며 객체의 퀄리티 설명
- CSEK ( Customer - Supplied Encryption Key ) : VM에 persistent disk(영구 디스크)를 첨가할 때 사용, 구글이 관리하는 것이 아닌 사용자가 직접 관리
- Object Versioning : 객체는 불변하기 때문에 대신 덮어쓰거나 삭제가 가능한데 이 덮어쓰거나 삭제에 대한 기록(로그)를 갖고 있어서 이전 상태로 복구하거나 객체를 영구적으로 삭제가 가능하다 (단, Bucket차원에서만 실행 가능)
- Object Lifecycle Management : 객체 live time 설정, 객체 버젼들(이전 상태들) 기록, 객체들의 Storage Class 다운그레이딩이 가능하다, 참고로 Lifecycle을 업데이트시, 새로운 버젼은 업데이트한 이후 24시간 뒤에 효력이 발휘된다.
- Object Change Notification : 객체가 업데이트되거나 삭제 시 알려주는 알림 서비스이다. ex) Web Hook ( 그러나, Cloud Pub/Sub이 더욱 효율적이고 빨라서 Pub/Sub이 더 권고된다.
- 거대량(테라바이트, 페타바이트)의 데이터를 업로드
  - Transfer Appliance : 대량의 데이터를 migrate(이전)하는 하드웨어 장치
  - Storage Transfer Service : 온라인 데이터의 고성능 import를 가능하게 한다.
  - Offline Media Import : 물리적인(Physical) 미디어(USB 드라이버, 하드디스크 드라이버)

### Sample1
- dependecy
````
org.springframework.cloud:spring-cloud-gcp-starter-storage
````
- application.properties keyfile 등록
  - json key file을 resources 폴더에 넣음
  - spring.cloud.gcp.storage.credentials.location=classpath:abc.json
  - key.json 파일에서 type, project_id, private_key_id, private_key 등 storage를 사용하는데 필요한 내용이 저장되어있음. Spring Boot에서 key 파일의 내용으로 stroage 변수에 자동으로 의존성을 부여
````
Blob blob = storage.get("버켓 이름", "버킷에서 다운로드할 파일 이름");
blob.downloadTo(Paths.get("로컬에 저장할 파일 이름"));
````

````
BlobInfo blobInfo = storage.create(BlobInfo.newBuilder("버켓 이름", "버켓에 업로드할 파일 이름")
.setAcl(new ArrayList<>(Arrays.asList(Acl.of(Acl.User.ofAllAuthenticatedUsers(), Acl.Role.READER))))
.build(),
new FileInputStream("로컬에서 업로드 할 파일이름"));
````

### Sample2
- application.properties
````
gcs-resource-test-bucket=[REPLACE_WITH_YOUR_BUCKET]
````
````
@RestController
public class WebController {

	@Value("gs://${gcs-resource-test-bucket}/my-file.txt")
	private Resource gcsFile;

	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String readGcsFile() throws IOException {
		return StreamUtils.copyToString(
				this.gcsFile.getInputStream(),
				Charset.defaultCharset()) + "\n";
	}

	@RequestMapping(value = "/", method = RequestMethod.POST)
	String writeGcs(@RequestBody String data) throws IOException {
		try (OutputStream os = ((WritableResource) this.gcsFile).getOutputStream()) {
			os.write(data.getBytes());
		}
		return "file was updated\n";
	}
}
````

https://cloud.google.com/storage/docs/concepts?hl=ko
https://cloud.spring.io/spring-cloud-gcp/multi/multi__spring_resources.html