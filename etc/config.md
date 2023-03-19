# mac ssh no matching key

error
````
no matching key exchange method found.
````
SSH 서버 구현에서 기본적으로 설정되는 알고리즘과 호환되지 않는 제한된 수의 MAC 및 kex(Key Exchange) 알고리즘을 사용
-> SSH 서버(예: 원격 호스트)에서 이전 키 교환(kex) 및 MAC 방법을 활성화

vi /etc/ssh/ssh_config
- 주석 제거
````
Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
MACs hmac-md5,hmac-sha1,umac-64@openssh.com
````

- 추가
````
HostkeyAlgorithms ssh-dss,ssh-rsa
KexAlgorithms +diffie-hellman-group1-sha1
````
