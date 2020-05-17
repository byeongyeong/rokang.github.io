iOS 프로젝트를 시작하면서 첫 커밋에 해야하는 것들
1. pod init, pod 'SwiftLint', pod update
2. .swiftlint.yml 파일 생성하고 SwiftLint rule 지정
3. Run script 추가(${PODS_ROOT}/SwiftLint/swiftlint)
4. https://www.gitignore.io/ 에서 'swift' 'xcode' 'cocoapods' 태그 달아서 만든 .gitignore 파일 추가하기
4-1. 수정된 .gitignore를 적용하고 싶다면 https://stackoverflow.com/questions/1139762/ignore-files-that-have-already-been-committed-to-a-git-repository 참고
5. README.md 생성

<스토리보드를 쓰지 않는다면>
6. Main.storyboard 삭제
7. Target에서 Main Interface에 지정된 Main 삭제
8. info.plist에서 Application Scene Manifest > Scene Configuration > Application Session Rule > Item 0 > Storyboard Name 삭제

<iOS Deployment Target이 13 이전이고 AppDelegate와 SceneDelegate를 둘 다 살리는 쪽>
9. AppDelegate didFinishLaunchingWithOptions 메서드에 
```
if #available(iOS 13.0, *) {
    // SceneDelegate에서 진행 됨
} else if #available(iOS 11.0, *) {
    let window = UIWindow(frame: UIScreen.main.bounds)
    self.window = window
    
    let launchVC = LaunchViewController()
    self.window?.rootViewController = launchVC
    self.window?.makeKeyAndVisible()
}
```
추가
10. SceneDelegate willConnectTo 메서드에
```
if let windowScene = scene as? UIWindowScene {
    self.window = UIWindow(windowScene: windowScene)
    let launchVC = LaunchViewController()
    self.window?.rootViewController = launchVC
    self.window?.makeKeyAndVisible()
}
```
추가