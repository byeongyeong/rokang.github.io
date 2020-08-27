---
layout: post
title: 스토리보드없이 iOS 프로젝트를 시작하려면?
tags:
  - Swift
  - iOS
---
스토리보드로 개발을 하다보면 merge conflict 문제가 큰 장애물로 다가올 때가 있습니다.
또 미천한 맥북 프로 8기가로는 스토리보드가 감당이 안되는 경우가 많죠. 😹
<br/>
그래서 저는 되도록 스토리보드없이 새로운 프로젝트를 진행하고있고 이 때 설정해야 할 것들을 간단하게 정리했습니다.

### 1. 기본 생성된 Main.storyboard 파일 삭제
### 2. Target에서 Main Interface에 지정된 Main 삭제(빈 상태로 만듬)
### 3. info.plist에서 Application Scene Manifest > Scene Configuration > Application Session Rule > Item 0 > Storyboard Name: Main 삭제
### 4. AppDelegate의 didFinishLaunchingWithOptions 메서드에 코드 추가
{% gist RoKang/86c78c9975b25be0efa74222f2d16933 %}
### 5. SceneDelegate의 willConnectTo 메서드에 코드 추가
{% gist RoKang/4291c34109ea5d3b0b17b52c6c4db0f2 %}

4번과 5번은 iOS Deployment Target이 13 이전이고 SceneDelegate를 살리기 위해서 작성하는 코드입니다. 
SceneDelegate는 iOS 13 이후부터 App의 UI 라이프사이클을 관리하기 위한 인스턴스이기 때문에 iOS 13 이후에는 SceneDelegate에서 UIWindow를 담당하게 됩니다.
만약, SceneDelegate를 사용하지 않고, 이전처럼 AppDelegate에서 UI Life Cycle까지 관리하고 싶다면 아래처럼 진행해주면 됩니다.

### 1. info.plist에서 Application Scene Manifest 삭제
### 2. SceneDelegate.swift 파일을 삭제한다
### 3. AppDelegate의 configurationForConnecting 메서드와 didDiscardSceneSessions 메서드 삭제

<br/>

피드백은 언제나 환영입니다 🔥