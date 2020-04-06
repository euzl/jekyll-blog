---
layout: post
title: GitHub | 기존 프로젝트 GitHub에 올리기
tags:
  - GitHub
  - 시행착오
---

_**안드로이드 스튜디오**에서 프로젝트를 GitHub에 올리는 방법이지만, 다른 기존 프로젝트들도 방법은 동일할 거라고 생각합니다._
<br>

## 1-1. VCS 연결하기 (1-2와 순서무관)
1) 안드로이드 스튜디오에서 `VCS - Enable Version Control Intergration...` 클릭
    <img width="297" alt="스크린샷 2020-04-06 오후 11 56 41" src="https://user-images.githubusercontent.com/37680108/78572336-4c45ea80-7862-11ea-802e-9965b6555a00.png">
<br>

2) Git으로 하겠다고 선택
![image](https://user-images.githubusercontent.com/37680108/78572500-844d2d80-7862-11ea-8e92-ca333c41fe8e.png)
<br>

## 1-2. GitHub에서 Repository 생성
<img width="713" alt="스크린샷 2020-04-06 오후 11 52 57" src="https://user-images.githubusercontent.com/37680108/78572709-c37b7e80-7862-11ea-88aa-91c5f71587e8.png"><br>

자유롭게 만들면 되지만, 위의 캡쳐와 같이 `Initialize this repository with a README`는 체크하지 않는 것이 좋다. 아래 캡쳐에 있는 안내문이 안 뜨기 때문에 혼란스러울 수 있기 때문이다. (이미 코드를 알고 있으면 상관없음)
<br>

<img width="792" alt="스크린샷 2020-04-06 오후 11 53 11" src="https://user-images.githubusercontent.com/37680108/78572918-089fb080-7863-11ea-96e0-fb9c5eeead9e.png"><br>

repository를 만들면 위의 화면이 뜬다! 
여기서 `...or push an existing repository from the command line`에 나와있는 명령어를 이용할 것이다. 
<br>
<br>

## 2. 안드로이드 프로젝트에서 remote설정 및 push
이제 안드로이드 프로젝트가 있는 폴더 terminal(또는 git)을 열어 다음 명령어 두 줄을 쳐주면 끝이다! 
*위의 캡쳐와 같이 깃헙에서 해당 repository에 적혀있음. 그대로 붙여넣기 하면 됨.

    $ git remote add origin https://github.com/<사용자아이디>/<레포이름>.git
    $ git push -u origin master

🛎주의할 점은 push이전에 commit 을 해줘야한다!! 그렇지 않으면... (아래에서 계속)
<br><br>


### 2-1. 혹시 이런 에러를 만나셨나요? (글을 쓴 목적)
<img width="328" alt="스크린샷 2020-04-06 오후 11 37 37" src="https://user-images.githubusercontent.com/37680108/78574020-5e288d00-7864-11ea-9e05-432312b839be.png">

    error: src refspec master does not match any.
    error: failed to push some refs to '~'

- 에러원인 : git을 초기화하고 **commit을 하지 않은 상태**에서 push를 시도해서
- 해결방법 : commit을 해주면 된다 ^!^


    $ git add . 
    $ git commit -m "first commit"
    $ git push -u origin master

(꼭 위의 코드와 같지 않아도 된다.)
<br><br>


## 3. 참조
- [안드로이드 깃허브 연동 & 올리기](https://gamjatwigim.tistory.com/74) 
- [\[문제해결\] error: src refspec master does not match any. ](https://m.blog.naver.com/PostView.nhn?blogId=kkson50&logNo=221322357858&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
- [\[Git\] git add 취소하기, git commit 취소하기, git push 취소하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html) ; 이번에 쓰이진 않았지만 유용할 듯
