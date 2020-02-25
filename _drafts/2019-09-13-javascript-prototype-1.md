---
layout: post
title:  "Javascript Prototype 개념"
date:   2019-09-12 07:07:07 +0900
categories: javascript prototype
---
본문 : [Medium post written by Rupesh Mishra][1]

링크 글에 있는 설명이 기가맥힘.

**HIGHLIGHT**: Prototype object of the constructor function is shared among all the objects created using the constructor function.
{:.message}

**NOTE**: 프로토타입의 프로퍼티를 재정의하면 프로토타입의 값이 바뀌는게 아니라 해당 객체가 독립된 프로퍼티를 생성하게 된다. 하지만 프로토타입의 프로퍼티가 Array라면 어떤 객체가 Array를 조작했을 땐 새로운 Array 프로퍼티가 객체에 생성되는게 아니라 프로토타입의 Array가 변한다.
{:.message}

### 결론
1. Constructor와 Prototype의 특성을 잘 이해해야 한다.
2. 객체 고유의 값은 Constructor로, 메모리 낭비를 방지하기 위해 객체끼리 공유할 수 있는 부분은 Prototype으로 설정하여 잘  조합해야 한다.


[1]: https://medium.com/better-programming/prototypes-in-javascript-5bba2990e04b