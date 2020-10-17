---
layout: post
title: Swift API Design Guidelines 톺아보기 - 2
tags:
  - Swift
---
이전 포스트에 이어서 Swift API Guidelines의 이름 지정(Naming)에 대해 톺아보겠습니다. 
개발을 하면서 많이 고민하는 부분 중 하나가 바로 이름을 짓는 것입니다. 많은 소스코드를 참고하고 영어 공부를 하는 것도 중요하겠지만,
좋은 이름을 짓기 위한 원칙과 컨벤션을 갖고있는 것 또한 매우 중요하기에 자세히 들여다봐야 할 부분입니다.

---

## Table of Contents
- [기본 개념(Fundamentals)](../swift-api-design-guideline/#fundamentals)
- [이름 지정(Naming)](./#naming)
    - [명확한 사용 활성화하기(Promote Clear Usage)](./#promote-clear-usage)
    - [유창하게 사용할 수 있도록 노력하기(Strive for Fluent Usage)](./#strive-for-fluent-usage)
    - [전문용어를 잘 사용하기(Use Terminology Well)](./#use-terminology-well)
- [규칙(Conventions)](../swift-api-design-guideline3/#conventions)
    - [일반적인 규칙(General Conventions)](../swift-api-design-guideline3/#general-conventions)
    - [매개 변수(Parameters)](../swift-api-design-guideline3/#parameters)
    - [인자 레이블(Argument Labels)](../swift-api-design-guideline3/#argument-labels)
- [특별 지침(Special Instructions)](../swift-api-design-guideline3/#special-instructions)

---

<h2 id="naming">이름 지정(Naming)</h2>







<h3 id="promote-clear-usage">명확한 사용 활성화하기(Promote Clear Usage)</h3>

- **코드의 모호성을 피하기 위해서 필요한 단어는 모두 포함시켜야 합니다.**
```swift
extension List {
    public mutating func remove(at position: Index) -> Element
}
/// 분명: x 인덱스에 있는 employee 제거
employees.remove(at: x)
```
```swift
/// 불분명: x를 제거하는가? x 인덱스의 값을 제거하는가?
employees.remove(x)
```
- **불필요한 단어는 생략합니다.** 단순히 타입을 반복하는 것은 줄이고, 최대한 매개변수의 역할을 설명하는 단어를 포함해야 합니다. 가끔은 타입의 반복이 모호성을 피하기 위해 필요할 수 있지만 역할로 표현하는 것이 더 좋습니다.
```swift
public mutating func removeElement(member: Element) -> Element?
allViews.removeElement(cancelButton) // 타입의 반복
```
```swift   
public mutating func remove(member: Element) -> Element?
allViews.remove(cancelButton) // 반복되는 타입을 줄임으로써 더 명확해짐.
```

- **역할로 표현되는 변수, 파라미터, 연관타입은 타입 제약사항보다 낫습니다.** 이름은 타입을 나타내기 보다는 그 entity의 역할, 정보를 나타내는 단어를 사용해야 합니다. 연관타입 이름 그 자체가 역할이 되는 프로토콜과 결합되어 있다면 뒤에 Type을 붙여서 충돌을 피해줍니다. 역할을 나타내는 entity를 다시 표현해야 하는 예외사항의 경우에는, 타입을 이름에 포함시켜주기도 합니다.
```swift
var string = "Hello" // string은 옳지 않아요.
protocol ViewController {
    associatedtype ViewType : View // Type을 굳이 반복하기 보다는 역할을 쓰는 것이 좋습니다.
}
class ProductionLine {
    func restock(from widgetFactory: WidgetFactory) // widgetFactory가 명확하지 않아요.
}
```
```swift
var greeting = "Hello" // 변수의 의미를 파악하기 쉽습니다.
protocol ViewController {
    associatedtype ContentView : View // View가 어떤 역할을 할지 느낌이 옵니다.
}
class ProductionLine {
    func restock(from supplier: WidgetFactory) // 매개변수 하나로 restock 메서드가 무엇인지 더 알기 쉬워졌습니다.
}
```
```swift
protocol Sequence {
    associatedtype IteratorType : Iterator // Iterator라는 프로토콜 이름 자체가 역할이기 때문에 연관타입 이름에 Type을 붙여서 충돌을 피해줍니다.
}
```

- **매개변수의 역할을 명확하게 할 수 있도록 약타입 정보를 보정해야 합니다.** 사용 시점에서 타입 정보와 상황이 의도를 완전히 전달하지 못할 수 있음을 기억해야 합니다. 선언은 명확하나 사용하는 곳이 막연해질 수 있는 것입니다. 자칫 역할이 모호해질 수 있는 약타입 매개변수들은 역할을 설명하는 명사를 외부 매개변수에 포함시켜서 사용 시점에서의 명확성을 높일 수 있습니다.
```swift
func add(observer: NSObject, for keyPath: String)
grid.add(self, for: graphics) // option+클릭으로 메서드를 직접 확인하지 않으면 무엇을 위해 add하는지 알기 어렵습니다.
```
```swift
func addObserver(_ observer: NSObject, forKeyPath path: String)
grid.addObserver(self, forKeyPath: graphics) // observer로 add하는 것이었군요!
```

<br>

> 🧑🏻‍💻 불필요한 단어는 제외하면서도 명확성을 높이기 위해서 필수적인 단어는 반드시 담아야한다는 게 쉽지 않은 것 같습니다. 
> 네이티브가 아니기에 모든 부분에서 완벽할 수는 없겠지만 항상 깔끔하게 코드를 유지할 수 있도록 신경써야겠습니다.
> 또, 이름을 작성할 때는 해당 부분이 사용 될 때 어떤 역할을 하게될 것인지 고민해야한다는 점은 명심해야겠습니다.

<br>







<h3 id="strive-for-fluent-usage">유창하게 사용할 수 있도록 노력하기(Strive for Fluent Usage)</h3>

- **메서드는 파라미터를 포함해서 문법상 영어 문구 형태로 작성하세요.** 봤을 때 영어 문장처럼 보일 수 있도록 자연스럽게 작성해야 합니다. 메서드에서 파라미터를 줄바꿈하는 케이스가 있는데 매개변수가 문장에서 중요한 역할을 하지 않을 때 그렇게 쓰기도 합니다.
```swift
// 좋은 예시
x.insert(y, at: z) // x에서 z에다 y를 삽입
x.subViews(havingColor: y) // 색상 y을 갖는 x의 subviews
x.capitalizingNouns() // x에 명사를 대문자화
```
```swift
// 별로인 예시
x.insert(y, position: z) // position이라는 단어의 의미가 애매합니다.
x.subViews(color: y) // 서브뷰를 만든다는건지 가져온다는건지 명확하지 않습니다.
x.nounCapitalize() // 역시나 와닿지 않습니다.
```
```swift
AudioUnit.instantiate(
    with: description, 
    options: [.inProcess], completionHandler: stopProgressBar)
```

- **팩토리메서드의 이름은 make prefix를 사용합니다.**
```swift
func makeIterator()
```

- **이니셜라이저와 팩토리 메소드 호출은 첫 번째 인자를 포함하지 않는 문구로 구성해야 합니다.** 영어 문장처럼 문법적 연속성을 만들지 않고 초기화 할 매개변수들을 단순 나열합니다.
```swift
// 좋은 예시
let foreground = Color(red: 32, green: 64, blue: 128)
let newPart = factory.makeWidget(gears: 42, spindles: 14)
```
```swift
// 별로인 예시
let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128) // 이렇게 연속성을
let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14) // 굳이 만들어 줄 필요가 없습니다.
```

- **사이드 이펙트에 따라 함수와 메소드 이름을 지정합니다.**

  -  사이드 이펙트가 없는 것은 `명사구로 읽어야 합니다.`
```swift
e.g. x.distance(to: y), i.successor()
```

  -  사이드 이펙트가 있는 것은 `반드시 동사 구문으로 읽어야 합니다.`
```swift
e.g. print(x), x.sort(), x.append(y)
```

  - Mutating/nonmutating 메소드 쌍을 일관되게 이름을 지정합니다. mutating 메소드는 종종 비슷한 의미가 있는 nonmutating variant가 있지만, `인스턴스 in-place를 갱신하는 것보다 새로운 값을 반환하는 것이 낫습니다.`

    - **연산자가 동사로 설명될 때**: mutating은 반드시 동사를 사용하고, nonmutating 메소드 이름을 지정하기 위해 “ed”나 “ing” 접미사를 적용

      |**mutating**|**nonmutating**|
      |:----------|:-------------|
      |`x.sort()`|`z = x.sorted()`|
      |`x.append(y)`|`z = x.appending(y)`|

      - 동사의 과거분사(보통은 `ed`를 붙임)를 사용하여 nonmutating variant 이름 지정을 선호합니다.
    ```swift
    mutating func reverse() // self의 in-place를 뒤집습니다
    func reversed() -> Self // 뒤집혀진 self의 사본을 반환합니다
    ```

      - 동사의 목적어가 있다면, 현재 분사를 사용하여 nonmutating variant에 `ing`을 붙여 이름을 지정합니다.
    ```swift
    mutating func stripNewlines() // self에서 모든 줄바꿈을 떼어냅니다.
    func strippingNewlines() -> String // 모든 줄바꿈을 떼어낸 self의 사본을 반환.
    ```

    - **연산자가 명사로 설명될 때**: mutating은 `form` prefix를 적용하고, nonmutating 메소드는 반드시 명사를 사용

      |**mutating**|**nonmutating**|
      |:----------|:-------------|
      |`y.formUnion(z)`|`x = y.union(z)`|
      |`y.formSuccessor(&i)`|`x = y.successor(i)`|

- Boolean 메소드와 속성 사용은 nonmutating일 때 수신자(아래 예시에서 x, line1)에 관한 주장으로 해석해야 합니다.
```swift
e.g. x.isEmpty, line1.intersects(line2)
```

- 무언가를 설명하는 프로토콜은 명사로 읽어야 합니다.
```swift
e.g. Collection
```

- 능력을 설명하는 프로토콜은 able, ible 또는 ing 접미사를 사용하여 이름을 지정해야 합니다.
```swift
e.g. Equatable, ProgressReporting
```

- 다른 타입, 속성, 변수 그리고 상수의 이름은 명사로 읽어야 합니다.

<br>

> 🧑🏻‍💻 자주 들여다봐야 할 부분이 많았습니다. 다양한 경우에 어떤 문법으로 작성해야 할지에 대한 내용이 많은 파트였습니다
> (글 쓰면서 영어 잘하시는 분들이 참 부러웠습니다 😭)

<br>







<h3 id="use-terminology-well">전문용어를 잘 사용하기(Use Terminology Well)</h3>

- Term of Art의 사전적 정의
```swift
📚 특정 영역이나 직업 내에서 정확하고 전문적인 의미가 있는 단어나 구.
```

- 더 일반적인 단어가 의미를 잘 전달할 수 있다면 애매한 용어는 피합니다. "피부(skin)"가 목적을 전달할 수 있다면 "표피(epidermis)"를 언급하지 않도록 합니다. `Terms of art는 주요한 커뮤니케이션 도구이지만, 손실될 중요한 의미를 포착하는 데 사용되어야 합니다.`

- `Term of Art`를 사용한다면 기존의 의미에 충실합니다.

  - 더 일반적인 단어보다 전문적인 용어를 사용하는 유일한 이유는 기술 용어가 좀 더 정확하게 표현해주기 때문입니다. 하지만 기술 용어는 애매하고 불분명해질 수 있다는 단점도 존재합니다. 그러므로 API는 받아들여지는 의미에 따라 엄격하게 용어를 선택하여 사용해야 합니다.

    - **전문가를 놀라게 하지 마세요.** 우리가 기존의 용어에 새로운 의미를 부여한 것처럼 보인다면, 그 용어에 익숙했던 사람들이 놀라거나 화를 낼 것입니다.

    - **초보자를 혼란스럽게 만들지 마세요.** 용어를 배우려는 사람들은 웹 검색을 하고 전통적인 의미를 찾으려고 할 것입니다.

- **약어를 피하세요.** 특히 표준이 아닌 약어는 비 축약 형태로 정확하게 번역될 수 있어야 효과적으로 Term of Art가 됩니다. (이 부분은 번역도 이해가 잘 가지 않아서 제 주관적으로 번역해보았습니다. 번역이 틀렸다면 댓글 달아주시면 감사하겠습니다)

  - 사용하는 약어에 의도된 의미는 웹 사이트에서 쉽게 찾을 수 있습니다.

- **선례를 받아들이세요.** 기존 문화에 적합성 비용에 있어 모든 초보자를 위해 용어를 최적화하지 마세요.

  - 연속 데이터 구조 이름은 `List처럼 간단한 용어 사용보다 Array가 낫습니다.` 비록 초보자도 쉽게 List의 의미를 파악할 수 있지만 말이죠. 현재 컴퓨팅에서 Array는 기초이고, 모든 프로그래머는 알고 있거나 Array가 무엇인지 곧 공부할 것입니다. 대부분 프로그래머가 잘 알고 있는 용어를 사용하고, 웹 검색과 질문으로 보상받을 것입니다.

  - 수학처럼 특정 프로그래밍 도메인 내에서 `verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)처럼 설명 문구보다 sin(x)처럼 넓게 사용되고 있는 선례 용어를 선호합니다.` 이 경우, 선례는 약어를 피하기 위한 가이드라인보다 중요합니다(전체 단어가 sine이지만, "sin(x)"은 수십 년간 프로그래머 사이에서, 수백 년간 수학자 사이에서 흔하게 사용되었습니다)

<br>

> 🧑🏻‍💻 어떤 단어를 선택할 것인지에 대한 부분이었습니다. 
> 의미를 잘 전달할 수 있으면서, 더 일반적인 단어를 선택하는 것이 좋다는 내용이었습니다.
> 전문 용어도 자칫 독이될 수 있기 때문에, 사용할 때는 그 전문 용어로 인해 API를 사용하는 사람들이 혼동하진 않을까, 이해하지 못하지는 않을까를 고민한 뒤에 선택해야겠습니다.
> 더 명확하게 설명할 수 있는 약어가 없을지, 약어가 충분히 내용을 설명할 수 있는지 등등 좋은 이름을 쓰기 위해 많은 고민이 필요하다는 것을 다시 한 번 느낍니다.

---




<br>

[다음 포스트](../swift-api-design-guideline3)에서 Swift API Guidelines 톺아보기를 마무리하겠습니다.

<br>

## 레퍼런스

- [[원문] Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [[번역] Swift API Design Guidelines](https://minsone.github.io/swift-internals/api-design-guidelines/)