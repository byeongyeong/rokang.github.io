---
layout: post
title:  "ssh에서 공개키 비밀키에 대한 공부"
date:   2019-09-19 07:07:07 +0900
categories: ssh key
---

리눅스 계열 컴퓨터는 서버, 클라이언트 ssh가 설치되어있다.

ssh-keygen으로 key를 생성할 수 있음. 

ssh-keygen -t rsa : rsa type의 암호화 방식으로 키를 생성(ssh-keygen의 디폴트 타입이 rsa인건 함정)
passphrase를 입력하면 ssh로 접속할때마다 비밀번호를 입력해야해서 보안상 좋지만 귀찮음

cd $HOME/.ssh 에 접근하면 id_rsa 파일과 id_rsa.pub 파일, known_hosts 파일이 있는데,
known_hosts는 접속한 적이 있는 서버들의 정보를 갖고 있음.
id_rsa 파일이 private key고 id_rsa.pub 파일이 public key임.

id_rsa는 식별자로서 보안상 매우 중요하기 때문에 permission을 보면 600(관리자만 읽고쓰기 가능)인걸 확인 가능.
id_rsa.pub은 공개키라 644인 것을 알 수 있음.

id_rsa.pub를 서버 /.ssh/authorized_keys 폴더에 복사하게 되면 로컬에서 ssh로 접속시 id_rsa(비밀키)와 붙여넣은 공개키를 비교해서 접속이 가능해짐.

파일 전송에는 scp(secure copy) 프로토콜을 사용(ssh, scp, sftp는 모두 22번 포트를 사용)하면 편하다.
나는 aws ec2를 사용중이기 때문에 scp 커맨드는 아래와 같음.
```scp -i ".pem path" ~/.ssh/id_rsa.pub <server>@<Public IP Address>:id_rsa.pub``` 는 서버 컴퓨터의 홈 디렉토리(~:)에 id_rsa.pub 라는 파일명으로 저장하게 됨.

서버 쉘에서 .ssh 폴더를 만들고 .ssh 폴더의 퍼미션을 700으로 설정함(관리자만 조작 가능하게)
.ssh 폴더 안에 authorized_keys 파일을 만들고 퍼미션을 600으로 설정함
```cat ~/id_rsa.pub >> ~/.ssh/authorized_keys``` 로 공개키를 authorized_keys 파일에 append
(cat 명령어는 정말 자주 쓰이니 링크 첨부[2])
```cat ~/.ssh/authorized_keys``` 으로 append 잘 됐는지 확인

이제 로컬에서 ```ssh <server>@<Public IP Address>```만 해도 접속가능해짐.

만약에 private key를 .ssh폴더 안에 있는 id_rsa 말고 다른 경로에 있는 걸로 사용하고 싶다면 ssh 프로토콜에 i옵션을 붙여서
해당 private key 경로를 지정해주면 됨. 단, 퍼미션을 안전하게(600) 설정해줘야 보안 문제가 없을 것임.

***SSH 접속로그를 자세히 보고 싶다면***
-v 옵션을 사용하면 디버깅 과정을 볼 수 있음.
-vv는 더 자세히 -vvv 더 더 자세히 볼 수 있음.

**FTP에 대한 내용은 따로 정리하자**
(참고로 ftp는 20, 21번 포트를 사용한다. ftp는 tcp 기반으로 평문 전송방식이다보니 보안성이 낮아서 ftps나 sftp를 사용함)
(ftp 프로토콜에 보안성을 추가한 프로토콜이 sftp(ssh 프로토콜 기반)와 ftps(tls 프로토콜 기반. tls는 ssl 기반.))
(active mode, passive mode 2가지 방식으로 ftp를 사용하는데 passive는 클라이언트가 서버에서 데이터를 가져가는 방식이라 보안에서 취약해 막아놓는 경우가 있다. acitve나 passive를 사용의 판단 여부는 '프로토콜'이 하는거지 '서버'가 하는게 아니다. passive를 막을 수 있는 주체는 '서버'지만.)
[1]참고

[1]: https://blogger.pe.kr/305
[2]: https://www.tecmint.com/13-basic-cat-command-examples-in-linux/