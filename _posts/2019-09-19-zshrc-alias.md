---
layout: post
title:  "zsh에서 alias로 편하게 살기"
date:   2019-09-19 07:07:07 +0900
categories: zsh
---

AWS EC2를 쓰다가 매번 터미널 켤 때마다 커맨드 입력하는게 비효율적이다 싶었음.
쉘 스크립트 찾아보다가 alias 사용하면 매우 간단하게 된다는 걸 알게 됨.
(bash를 쓴다면 zshrc를 bashrc로 치환하면 됨)

<br>

**순서**
1. ```vi ~/.zshrc```
2. ```alias [name]='cd [path]; ssh -i [key] [server DNS];'``` 추가
3. ```source ~/.zshrc``` 로 커맨드 적용
4. 터미널에서 [name] 입력하면 사용 가능

<br>

**블로그를 위해 blog alias와 blog_push alias도 추가**

<br>

> zshrc에서 rc가 뭘까? <br>
> A : It stands for “run commands”. (중간 생략) rc stuck as a name for any list of commands. <br><br>
> _(superuser.com의 답변 중에서)_
{:.message}