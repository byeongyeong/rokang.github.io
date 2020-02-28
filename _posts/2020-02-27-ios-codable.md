---
layout: post
title: Codable과 JSON
tags:
  - Swift
  - iOS
---
## Why?
새로운 API와 함께 도착한 JSON 데이터를 처리하려다 보니 잠시 뇌가 멈췄습니다.

처리하려는 JSON 데이터의 구조는 대충 이런 형태였습니다.

```swift
{
  ...,
  "data": {
    "room": [
      {
        "name": "room1",
        "order": 1,
        "messages": [
          {
            "content": "Mom",
            "person": "Tom",
            "order": 1
          },
          {
            "content": "I'm sorry",
            "person": "Will",
            "order": 2
          },
          {
            "content": "Thank you",
            "person": "Big brother Tom",
            "order": 3
          }
        ]
      },
      ...
    ]
  }
}
```
JSONSerialization 클래스로 Foundation 객체를 만들고~ 만든 Dictionary로 key값 불러오고~
열심히 디코딩한 결과는
```swift
if let data = jsonString.data(using: .utf8) {
    if let decoded = try? JSONSerialization.jsonObject(with: data, options: []) {
        let dataDic = (decoded as? [String: Any])?["data"] as? [String: Any] ?? [:]
        let roomArray = dataDic["room"] as? [[String: Any]] ?? []

        for room in roomArray {
            print(room["name"])
            print(room["order"])
            
            let messages = room["messages"] as? [[String: Any]] ?? []
            
            for msg in messages {
                print(msg["content"])
                print(msg["person"])
                print(msg["order"])
            }
        }
    }
}
```
...... 하하하

개비스콘이 필요한 이 코드를 그냥 두고 볼 수 없었던 저는 구글링을 시작했고
Swift4에서 `Codable` 이라는 프로토콜을 지원하는 것을 발견하게 되었습니다!
대충 봐도 안 쓸 수가 없는 프로토콜이어서 JSON 인코딩과 디코딩에 적용하기로 마음 먹었죠.

## Codable?
[Codable](https://developer.apple.com/documentation/swift/codable)은 Decodable Encodable을 포함하는 `protocol`이라고 공식 문서에 나와있네요. [Encoding and Decoding Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)를 보게 되면, Swift는 데이터를 인코딩하고 디코딩하는 표준화된 접근법을 정의하고 있다는 내용이 있습니다.

그럼 이제 Codable을 이용해서 저 괴물같은 디코딩 코드를 좀 아름답게 바꿔보도록 하죵.
```swift
struct Result: Decodable {
    var success: Bool?
    var reason: String?
    var rooms: [Room]?
    
    enum CodingKeys: String, CodingKey {
        case success, reason, rooms = "room"
        case data = "data"
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let data = try container.nestedContainer(keyedBy: CodingKeys.self, forKey: .data)
        
        success = try container.decode(Bool.self, forKey: .success)
        reason = try container.decode(String.self, forKey: .reason)
        rooms = try data.decode([Room].self, forKey: .rooms)
    }
}

struct Room: Codable {
    var name: String?
    var order: Int?
    var messages: [Message]?
    
    enum CodingKeys: String, CodingKey {
        case name, order, messages
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
        order = try container.decode(Int.self, forKey: .order)
        messages = try container.decode([Message].self, forKey: .messages)
    }
}

struct Message : Codable {
    var content: String?
    var person: String?
    var order: Int?
    
    enum CodingKeys: String, CodingKey {
        case content, person, order
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        content = try container.decode(String.self, forKey: .content)
        person = try container.decode(String.self, forKey: .person)
        order = try container.decode(Int.self, forKey: .order)
    }
}

let data = jsonString.data(using: .utf8) ?? Data()
if let result = try? JSONDecoder().decode(Result.self, from: data) {
    print("success : \(result.success!)")
    print("reason : \(result.reason!)\n")
    
    for room in result.rooms! {
        print("  name : \(room.name!)")
        print("  order : \(room.order!)\n")
        
        for msg in room.messages! {
            print("    content : \(msg.content!)")
            print("    person : \(msg.person!)")
            print("    order : \(msg.order!)\n")
        }
    }
}
```
코드는 더 길어졌지만 json 데이터로부터 Result, Room, Message 구조체 인스턴스를 추출해서 데이터를 더 깔끔하게 처리할 수 있게 되었습니다! 짝짝짝

아ㅏ 참고로, 예시이다보니 print에서 인스턴스 변수를 강제 언래핑으로 처리했지만, 실제 json 데이터는 nil일 수 있기 때문에 if 구문이나 guard를 사용하시는게 더 안전합니다.

코드를 다시 보면 CodingKey라는 낯선 친구가 보입니다. 이 친구와 함께 `init(from decoder: Decoder)`가 어떻게 동작하는지 간단히 살펴보겠습니다.

## CodingKey?
[CodingKey](https://developer.apple.com/documentation/swift/codingkey)는 인코딩과 디코딩을 위한 키로 쓰이는 타입이라고 하네요. 역시나 프로토콜입니다. 많은 분들은 이미 감을 잡으셨을 겁니다. 예. json의 key입니다. 코드를 다시 보시면 CodingKey 프로토콜을 채택한 CodingKeys enum이 json 데이터의 key와 매핑되는 것을 알 수 있습니다.

지금까지 디코딩에 대한 부분만 보여드렸지만 인코딩은 JSONEncoder 클래스를 활용해서 인스턴스를 json data로 변환할 수 있다는 점도 알아두면 좋을 것 같습니다.

## 팁?
1. 바닐라 Codable도 JSONSerialization에 비해서 상당히 편해졌지만, [Codextended](https://developer.apple.com/documentation/swift/codingkey) 라는 오픈소스는 더~ 쉽게 인코딩과 디코딩을 할 수 있게 해줍니다. Star 수도 1,100이고 커밋도 비교적(?) 최근이어서 json 데이터를 더 쉽게 다루고자 하신다면..! 유용할 듯 합니다. 저는 아직 미숙해서... 껄껄

2. 날짜 json data를 Date 타입으로 포맷팅하는건 귀찮은 일이지만, 매우 중요합니다. 그래서..! Codable에는 별다른 포맷팅없이 어떤 데이터가 특정 날짜 포맷에 맞다면 자동으로 Date 디코딩해주는 strategy(전략)를 구사합니다 키키.
JSONDecoder().dateDecodingStartegy에 위에서 말한 '특정 날짜 포맷' 을 지정할 수 있습니다. 기본으로 제공되는 전략(포맷)도 있지만, 유저가 지정한 DateFormatter를 전략으로 쓸 수도 있고 클로져를 사용한 커스터마이징 전략도 가능합니다.
음... 저는 formatted를 많이 사용하게 될 것 같네요.