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

### Sample
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
https://cloud.google.com/storage/docs/concepts?hl=ko
