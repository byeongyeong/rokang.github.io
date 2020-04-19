---
layout: post
title: Swift - JSON과 Codable
tags:
  - Swift
  - iOS
  - JSON
  - Codable
---
**Codable 프로토콜로 JSON 데이터 처리하기**

## Why?
새로운 API와 함께 도착한 JSON 데이터를 처리하려다 보니 잠시 뇌가 멈췄습니다.

JSON 구조는 대충 이랬습니다.

<!-- JSON 예제 코드 -->
{% gist RoKang/cf87c669c296a5392395a7e1eb0f9561 %}

정신 차리고 원래 해왔던 것처럼 열심히 디코딩한 결과는...

<!-- 하드하게 디코딩한 코드 -->
{% gist RoKang/550a3febcce5421f7020a9ef5185c389 %}

...... 하하하

개비스콘 없이는 소화하기 힘든 코드네요 껄껄. 이 코드를 그냥 둘 수는 없으니 검색을 해봐야겠습니다. 스위프트 고수 분들은 어떻게 쓰는지..

아 Swift4에 `Codable` 이라는 게 생겼군요. 일단 이게 뭔지부터 좀 알아봐야겠습니다.

## Codable?
[Codable](https://developer.apple.com/documentation/swift/codable){:target="blank"}은 Decodable Encodable을 포함하는 `protocol`이라고 공식 문서에 나와있네요. [Encoding and Decoding Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){:target="blank"}를 보게 되면, Swift는 데이터를 인코딩하고 디코딩하는 표준화된 접근법을 정의하고 있다는 내용이 있습니다. 표준이라는데 당연히 써줘야죠.

그럼 이제 Codable로 저 괴물같은 디코딩 코드를 나름대로 좀 아름답게 바꿔보도록 하겠습니다.

<!-- Codable 적용한 구조체 코드 -->
{% gist RoKang/e665070d5d6d6b873446e217e2ea8072 %}

<!-- Codable이 적용된 구조체로 디코딩하는 예제 코드 -->
{% gist RoKang/5a0e1026e5313142c2db2a216bea93d3 %}

코드는 더 길어졌지만 json 데이터로부터 Result, Room, Message 구조체 인스턴스를 추출해서 데이터를 더 깔끔하게 처리할 수 있게 되었습니다! 짝짝짝

아 참고로, 예시이다보니 print에서 인스턴스 변수를 강제 언래핑했지만, 실제 json 데이터는 nil일 수 있기 때문에 if 구문이나 guard를 사용하시는게 더 안전합니다.

코드를 다시 살펴보면 CodingKey라는 낯선 친구가 보입니다. 이 친구와 함께 `init(from decoder: Decoder)`가 어떻게 동작하는지 간단히 살펴보겠습니다.

## CodingKey?
[CodingKey](https://developer.apple.com/documentation/swift/codingkey){:target="blank"}는 인코딩과 디코딩을 위한 키로 쓰이는 타입이라고 하네요. 코드를 다시 보시면 CodingKey 프로토콜을 채택한 CodingKeys enum의 값들이 json 데이터의 key와 매핑되는 것을 알 수 있습니다. 예를 들어, Result의 rooms 데이터는 디코딩을 위해 "room" 키로 매핑되어있네요.

그럼 이 코드가 어떻게 동작하는지 한번 살펴보도록 해요.

1. decode 메서드는 디코딩 될 data로부터 Result 타입의 value를 만들어줍니다.
<!-- 표준 라이브러리 코드 1 -->
{% gist RoKang/3f1252e502a98ce31801600d325a9d32 %}


2. 1번 코드가 Result를 초기화하는 init 코드를 실행하겠네요. decoder에 담겨있던 data로부터 container를 생성해줍니다. `CodingKeys.self` 타입이 data의 key와 매핑이 되지 않는다면 `DecodingError.typeMismatch` 에러를 던지겠네요.
<!-- 표준 라이브러리 코드 2 -->
{% gist RoKang/c1e77fd8d79dc5816d67cd85b08ea030 %}


3. nested이기 때문에 forKey 파라미터가 추가되서 해당 키에 대한 sub container를 생성해줍니다.
<!-- 표준 라이브러리 코드 3 -->
{% gist RoKang/0c1a401def1d28b496f0bf3947560525 %}


4. sub container인 data 인스턴스에서 "room" key에 해당하는 데이터를 [Room] 타입의 value로 디코딩 해줍니다.
Room 타입도 구조체이기 때문에 이 과정에서 Room의 init도 실행이 되겠네요.
<!-- 표준 라이브러리 코드 4 -->
{% gist RoKang/a56af6bb733cf19bebbbf177f460d831 %}

지금까지의 내용이 디코딩만 다루고 있지만, 인코딩은 JSONEncoder 클래스를 활용한 반대의 과정이니까 식은 죽 먹기 아닐까 싶네요 크크

## 팁
1. 🔥 바닐라 Codable도 JSONSerialization에 비해서 상당히 편해졌지만, [Codextended](https://github.com/JohnSundell/Codextended){:target="blank"} 라는 오픈소스는 더 쉽게 인코딩과 디코딩을 할 수 있게 해줍니다. 깃헙의 Star 수도 1,100이고 커밋도 비교적(?) 최근이어서 Codable도 귀찮다! 더 쉽게 json을 다루고 싶다! 하신다면 유용하지 않을까 싶습니다. 저는 아직 애송이라 나중에 고려해봐야겠네요ㅋㅋ

2. 📆 날짜 json data를 Date 타입으로 포맷팅하는건 귀찮지만 중요한 일이죠. 그래서 Codable에는 어떤 날짜 데이터가 특정 포맷에 맞다면 `자동으로 Date 인스턴스로 디코딩` 해주는 strategy(전략)이 포함되어 있습니다.
<!-- JSONDecoder().dateDecodingStrategy 예제 코드 1 -->
{% gist RoKang/fa262e9d404d05cf4c7938978fbf7043 %}

이렇게 직접 작성한 DateFormatter를 지정해서 yyyy.MM.dd 포맷의 데이터를 Date로 변환할 수도 있구요

<!-- JSONDecoder().dateDecodingStrategy 예제 코드 2 -->
{% gist RoKang/73fad2d1c259f0d3208c6478621be5f5 %}

이렇게 표준화 된 포맷을 적용할 수도 있습니다!
저는 아무래도 formatted나 iso8601을 많이 사용하지 않을까 싶네요.