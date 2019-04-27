---
layout: post
title:  "Apple iOS 개발자 계정 만들기"
date:   2019-04-27 00:00:07 +0900
categories: ios Apple
---

Apple App Store에 앱을 등록하기 위해서는 Apple Developer 계정이 필요합니다.  
이번 글에서는 Apple Developer 계정을 생성하는 방법을 정리하겠습니다.

### 준비하기

* 신용카드
* Apple 계정
* 영문 주소 [(영문 주소 변환 사이트)][1]

### 진행하기

* [Apple Developer Program Enroll][2] 를 클릭하면 Developer 계정을 만들기 위해 필요한 내용이 나옵니다.  
A D-U-N-S® Number는 dun&bradstreet에서 발급하는 국제사업자등록번호로 회사 법인을 증명하기 위한  
번호입니다. Apple Developer 계정을 회사 계정으로 등록하기 위해서는 D-U-N-S 번호가 필수입니다.  


**NOTE**: 저처럼 1인 개발자이거나 개인 비즈니스를 운영하시는 분이라면 D-U-N-S가 필요없습니다. 회사 계정이 필요하신 분들은 [링크][3]혹은 구글에서 검색하면 D-U-N-S 번호를 쉽게 발급받으실 수 있을 겁니다. 참고로 D-U-N-S 발급을 포함한 Developer 계정 생성에 1달까지도 소요된다고 하니 기간을 넉넉하게 잡으시는게 정신건강에 좋을 것 같습니다.
{:.message}


<br/>![Apple Developer Below]({{site.baseurl}}/assets/img/docs/apple-developer-enroll/below.png)


* 페이지 하단에 있는 Start Your Enrollment 버튼을 클릭하면 로그인 페이지가 나올텐데 Apple Developer로 등록하기 위한 `Apple 계정`을 입력하면 됩니다.<br/>


* Entity Type(유형)은 `Invidiual / Sole Proprietor / Single Person Business` 를 선택하면 됩니다. 회사 계정이신 분들은 `Company / Organization` 을 클릭하면 됩니다.


* 진행하다 보면 주소를 입력하는 부분이 있습니다. 처음에 준비한 영문 주소로 작성하면 됩니다.
	* Postal Code : 우편번호<br/>
	* State / Province : -도 (ex. 경기도, 충청도, 서울특별시)<br/>
	* Town / City : -시 (ex. 서울시, 수원시)<br/>
	* Address Line 1, 2 : 나머지 상세주소


* 등록을 완료하면 1년동안 사용 가능한 Developer 계정 ID를 받게 됩니다. 준비한 신용카드로 결제를 완료하면 
Apple에서 최종 승인을 해줍니다(시간이 좀 걸릴 수 있습니다. 저 같은 경우는 하루 정도 소요되었습니다)


* 승인이 완료되면 [Apple Developer Account Welcome][4]에서 아래와 같은 화면을 보실 수 있을 겁니다.


<br/>![Apple Developer Account Login Page]({{site.baseurl}}/assets/img/docs/apple-developer-enroll/login.png)


### 마무리
여기까지 Apple Developer 계정을 생성해 보았습니다. 다음 편에서는 Xcode에 Apple Developer를 등록하고 iOS App을 아이폰에서 실행하는 방법을 포스트하겠습니다. 도움이 되셨기를 바랍니다.


[1]: http://www.jusoen.com/
[2]: https://developer.apple.com/programs/enroll/
[3]: https://m.blog.naver.com/PostView.nhn?blogId=watchmafia&logNo=220669940824&proxyReferer=https%3A%2F%2Fwww.google.com%2F
[4]: https://developer.apple.com/account/#/welcome