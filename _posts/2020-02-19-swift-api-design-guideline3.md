---
layout: post
title: Swift API Design Guidelines 톺아보기 - 3
tags:
  - Swift
---
이전 포스트에 이어서 Swift API Guidelines의 규칙(Conventions)과 특별 지침(Special Instructions)에 대해 톺아보겠습니다.
명확한 API를 작성하기 위한 다양한 규칙을 확인하고 몇가지 특별 사항을 파악한 뒤에 Swift API Guidelines 톺아보기를 마무리짓도록 하겠습니다.

---

## Table of Contents
- [기본 개념(Fundamentals)](../swift-api-design-guideline/#fundamentals)
- [이름 지정(Naming)](../swift-api-design-guideline2/#naming)
    - [명확한 사용 활성화하기(Promote Clear Usage)](../swift-api-design-guideline2/#promote-clear-usage)
    - [유창하게 사용할 수 있도록 노력하기(Strive for Fluent Usage)](../swift-api-design-guideline2/#strive-for-fluent-usage)
    - [전문용어를 잘 사용하기(Use Terminology Well)](../swift-api-design-guideline2/#use-terminology-well)
- [규칙(Conventions)](./#conventions)
    - [일반적인 규칙(General Conventions)](./#general-conventions)
    - [매개 변수(Parameters)](./#parameters)
    - [인자 레이블(Argument Labels)](./#argument-labels)
- [특별 지침(Special Instructions)](./#special-instructions)

---

<h2 id="conventions">규칙(Conventions)</h2>

<h3 id="general-conventions">일반적인 규칙(General Conventions)</h3>

- **O(1)이 아닌 계산 속성의 복잡성은 문서로 만들어줍니다.** 사람들은 속성 접근이 중요한 계산을 수반하지 않는다고 종종 가정하는데, 이는 사람들이 속성들을 mental model과 같이 저장하기 때문입니다. 그 가정이 어긋날 수 있을 때 그들에게 경고해 주어야 합니다.

- **자유 함수(free functions)보다는 메서드와 속성을 선호합니다.** 자유 함수는 특별한 경우에만 사용됩니다. 그 특별한 경우는 아래와 같습니다.

  1. 명백한 self가 없을 때
  ```swift
  min(x, y, z)
  ```

  2. 함수가 제약되지 않은 제네릭일 때
  ```swift
  print(x)
  ```

  3. 함수 구문이 기존 도메인 표기의 일부일 때
  ```swift
  sin(x)
  ```

- **case convention을 따릅니다.** 타입과 프로토콜의 이름은 UpperCamelCase이고 그 이외의 모든 것은 lowerCamelCase입니다.

  - 미국식 영어에서 모두 대문자로 흔하게 표시하는 [두문자어(Acronym과 initialism)](https://en.wikipedia.org/wiki/Acronym)는 case convention을 따라 대문자이거나 소문자로 일관되어야 합니다.
  ```swift
  var utf8Bytes: [UTF8.CodeUnit] // utf8, UTF8로 소문자와 대문자 일관
  var isRepresentableAsASCII = true // ASCII로 대문자 일관
  var userSMTPServer: SecureSMTPServer // SMTP로 대문자 일관
  ```

  - 다른 약어(acronym)는 일반적인 단어로 다뤄져야 합니다.
  ```swift
  var radarDetector: RadarScanner // radar는 약어
  var enjoysScubaDiving = true // Scuba도 약어
  ```

- **메서드는 같은 기본 의미를 공유할 때 또는 구분된 도메인에서 수행할 때 기본 이름을 공유할 수 있습니다.**

  - 다음은 권장하는 예제로, 메소드는 본질적으로 같은 것을 수행할 수 있기 때문입니다.
  ```swift
  extension Shape {
      // 3가지 메서드 모두 파라미터인 other가 self 지역 내에 있는 경우 true를 반환합니다
      func contains(other: Point) -> Bool { ... } 
      func contains(other: Shape) -> Bool { ... }
      func contains(other: LineSegment) -> Bool { ... }
  }
  ```

  - 그리고 기하학 타입 또는 컬렉션은 별도의 도메인이기 때문에, 같은 프로그램 내에서 contains 메서드를 중복해서 사용해도 괜찮습니다.
  ```swift
  extension Collection where Element: Equatable {
      // self가 sought와 같은 요소를 포함하는 경우 true를 반환힙니다
      func contains(sought: Element) -> Bool { ... }
  }
  ```

  - 그러나 index 메소드가 다른 의미가 있다면 반드시 다른 이름으로 만들어져야 합니다.
  ```swift
  extension Database {
      // DB 검색 index를 rebuild 함
      func index() { ... }

      // 주어진 Table에서 n번째 row를 반환합니다
      func index(n: Int, inTable: TableID) -> TableRow { ... }
  }
  ```

  - 마지막으로, "반환 타입에 오버로딩" 하는 것을 피하세요. **이는 타입 추론이 있을 때 모호성을 일으키기 때문입니다.**
  ```swift
  extension Box {
      // self에 저장된 Int를 반환하고, 없으면 nil을 반환합니다
      func value() -> Int? { ... }

      // self에 저장된 String을 반환하고, 없으면 nil을 반환합니다
      func value() -> String? { ... }
  }
  ```

<br>

> 🧑🏻‍💻 fdsaasff

<br>







<h3 id="parameters">매개 변수(Parameters)</h3>



<h3 id="argument-labels">인자 레이블(Argument Labels)</h3>

<h2 id="special-instructions">특별 지침(Special Instructions)</h2>




---




<br>

가이드라인을 살펴보면서 낸 결론 및 느낌.

<br>

## 레퍼런스

- [[원문] Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [[번역] Swift API Design Guidelines](https://minsone.github.io/swift-internals/api-design-guidelines/)