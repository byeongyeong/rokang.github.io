---
layout: post
title:  "Slack 채널에 Trello 보드 연동하기"
date:   2019-09-17 07:07:07 +0900
categories: trello slack
---

별 거 없는데 나중에 또 같은 일을 해야한다면 시간을 줄이기 위해서 씀.
협업 진행을 위해 slack과 trello를 사용하려했는데 두 개를 연동 가능하다는 것을 알게 됐음.

<br>

**순서**
1. slack에서 trello 앱을 설치
2. trello에서 팀을 하나 만들고 slack에서 공유할 보드 하나를 생성함(팀으로만 slack 공유가 가능함. 개인으로 안됨.)
3. [Add to Slack][1] 클릭에 클릭에 클릭을 걸치면 선택한 trello 팀이 slack의 workspace에 연결됨. 링크가 안 뜬다면 trello 팀 setting에 들어가면 settings 탭이 있는데 'Slack Team Linking' 부분 참고하면 됨.
4. trello와 연동하고 싶은 slack 채널을 하나 생성함(보통 프로젝트 이름)
5. 만든 채널에서 `/trello setup` 입력하면 slack에서 trello를 관리,조작 할 수 있는 권한을 부여함.
6. `/trello link` [채널에서 공유할 보드 링크] 를 땅 누르면 보드 리스트가 나옴.
7. `/trello help` 로 다양한 명령어 확인해서 유용하게 쓰자.

<br>

**Trello Alerts**
> Trello에서 리스트가 생기는 등의 이벤트가 발생할 때 slack에 알리기 위함

1. slack과 연동한 trello 보드에 들어가서 Show Menu > Power-Ups를 클릭하면 여러 익스텐션들이 나오는데 slack 검색.
2. slack 추가하면 Show Menu 버튼 옆에 Slack 버튼이 생성됨. 그거 클릭하면 Add Slack Alert... 버튼 있음.
3. 그거 누르고 또 뭐 권한 부여하는 단계 있는데 다 진행하고 Add Slack Alert... 버튼 다시 클릭.
4. 알림 보내고 싶은 채널이랑 이벤트 종류 선택하고나면(난 전부 다 선택)...
5. `DONE`
6. 테스트해보니 잘 됨.

<br>

> ... 제출 후에도 수정 쌉가능! ㅎㅇㅌ :) <br>
> _(LG CNS 인재확보팀의 Web 발신 문자 중에서)_
{:.message}


[1]: https://trello.com/platforms/slack
