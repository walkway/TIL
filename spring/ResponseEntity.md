# ResponseEntity

- 결과 데이터, HTTP 상태코드 제어 가능한 클래스
- statusCode, headers, body
- 반환 값인 경우 @ResponseBody 설정 x

### spring boot download file
````
@RequestMapping(path = "/download", method = RequestMethod.GET)
public ResponseEntity<Resource> download(String param) throws IOException {
    Path path = Paths.get(System.getProperty("user.dir") + "test.txt");
    ByteArrayResource resource = new ByteArrayResource(Files.readAllBytes(path));

    return ResponseEntity.ok()
            .contentType(MediaType.parseMediaType("application/octet-stream"))
            .body(resource);
}
````