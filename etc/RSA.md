# RSA

- 공개키와 개인키를 세트로 만들어서 암호화와 복호화를 하는 인터넷 암호화 및 인증 시스템
- 사용자가 로그인 폼을 채우면 사용자 ID와 비밀번호를 RSA 공개키로 암호화해서 전송하여, 중간에 패킷을 가로채도 해석이 불가능하게 만드는 것이다.
- 기본 작동 원리
 - [서버] 서버측에서 RSA 공개키와 개인키(비밀키)를 생성하여, 개인키는 세션에 저장하고 공개키는 자바스크립트 로그인 폼이 있는 페이지로 출력한다.
 - [클라이언트] 로그인폼은 자바스크립트가 실행되지 않는 환경에서는 발행(submit)이 되면 안된다.
 - [클라이언트] 로그인폼에 사용자의 ID와 비밀번호를 넣고 발행을 누르면 자바스크립트가 이를 가로챈다.
 - 사용자 ID를 RSA로 암호화하여 화면에 안보이는 새로운 폼에 저장한다.
 - 비밀번호를 RSA로 암호화하여 화면에 안보이는 새로운 폼에 저장한다.
 - 화면에 안보이는 해당 폼을 서버로 발행한다.
 - [서버] 서버측에서 세션에 저장된 RSA개인키로 암호화된 사용자ID와 비밀번호를 복호화한다.
 - [서버] 데이터베이스/혹은 기타 저장소에 저장된 사용자 ID와 비밀번호가 일치하는지 확인한다.
 - 암호화 된 값은 byte 배열이다. 이를 문자열 폼으로 전송하기 위해 16진 문자열(hex)로 변경한다. 서버측에서도 값을 받을 때 hex 문자열을 받아서 이를 다시 byte 배열로 바꾼 뒤에 복호화 과정을 수행한다.
````
HttpSession session = request.getSession();

KeyPairGenerator generator;
try {
    generator = KeyPairGenerator.getInstance("RSA");
    generator.initialize(1024);

    KeyPair keyPair = generator.genKeyPair();
    KeyFactory keyFactory = KeyFactory.getInstance("RSA");
    PublicKey publicKey = keyPair.getPublic();
    PrivateKey privateKey = keyPair.getPrivate();

    session.setAttribute("RSA_WEB_KEY", privateKey);
    RSAPublicKeySpec publicSpec = (RSAPublicKeySpec) keyFactory.getKeySpec(publicKey, RSAPublicKeySpec.class);
    String publicKeyModulus = publicSpec.getModulus().toString(16);
    String publicKeyExponent = publicSpec.getPublicExponent().toString(16);

    request.setAttribute("RSAModulus", publicKeyModulus);
    request.setAttribute("RSAExponent", publicKeyExponent);
````
- http://kwon37xi.egloos.com/4427199