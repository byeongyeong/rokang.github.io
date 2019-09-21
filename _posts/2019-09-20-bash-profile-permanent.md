---
layout: post
title:  "nodejs에서 process.env로 환경변수 사용하기"
date:   2019-09-20 07:07:07 +0900
categories: nodsjs bash zsh
---

nodejs에서 RDS에 접근하기 위한 설정 내용을 입력해야하는데 하드코딩하는 것은 문제가 있다고 판단(실제 서비스는 더더욱 그래서는 안되겠지)

그래서 쉘에 환경변수로 설정 내용을 등록하고 nodejs에서 process.env 객체로 접근하는 방식을 사용하기로 함.

**그런데..?**

.bash_profile에 환경변수를 등록해도 process.env에서 출력이 안됨.
구글링을 통해 문제를 파악함.

먼저, bash_profile, bashrc를 제대로 알고 있지 않아서 개념 정리를 따로 함.

### Login Shell
계정과 암호를 입력해서 Shell을 실행하는 것. ssh로 접속하거나 GUI를 통해서 Shell을 실행하는 경우

### Non-Login Shell
말 그대로 로그인 없이 Shell을 실행하는 것. ssh로 접속 후 다시 bash를 실행하는 경우나 새 탭으로 터미널을 띄우는 경우

### .bashrc
Non-Login Shell에서 실행된다.

### .bash_profile
Login Shell에서 실행된다. 어떤 shell 프로그램이던 .profile은 로드되지만 .bash_profile은 bash shell로 로그인 할 때만 실행된다.

<br>

>
**NOTE** 단, MAC OS 에서는 로그인 여부 관계 없이 모든 터미널을 Login Shell로 실행한다. 즉 모든 bash 터미널은 실행될 때 .bash_profile을 로드하는 것. 그런데 왜 process.env에서 출력되지 않았을까?<br><br>
**A) 내가 쓰던 쉘이 zsh여서...** <br><br>
**_해결방법_**
* .zshrc에 source ~/.bash_profile을 추가해서 zshrc가 로드될 때마다 .bash_profile을 로드하기
* 그냥 .zshrc에 다 때려넣기(이게 나아보인다)

#### 궁금했던 것
bashrc가 Non-Login Shell에서 실행되면 zshrc도 마찬가지아니냐?
#### 해결
zshrc는 zsh_profile로 구분되어 있지 않음. zshrc는 로그인 비로그인 상관없이 항상 실행된다는군요.