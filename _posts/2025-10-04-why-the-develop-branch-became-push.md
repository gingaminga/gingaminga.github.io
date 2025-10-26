---
title: "[GitHub] 왜 develop 브랜치에 push가 되었는가"
categories: [GitHub]
render_with_liquid: false
---

특정 상황을 제외하고는 PR을 통해서만 `develop`, `main`에 merge 할 수 있다.  
나는 주로 VSCode(Cursor)에서 작업하고, IDE를 사용해 add/commit/push를 한다.  
그런데 이번에도 동일하게 push 했는데, 왜 `develop` 브랜치에 push가 됐을까?

원인을 찾진 못했지만.. 트래킹한 내용을 남겨놓으려고 한다!

## VSCode에서의 Git 로그

![](https://velog.velcdn.com/images/gingaminga/post/840ff55d-e348-4a93-b2ac-0379892e1372/image.png)

다행히 VSCode에서 git 관련 로그를 캡쳐해놨다!  
push 할 때 명령어가 왜 `git push origin fix/blabla:develop`으로 된걸까?

## 기억 더듬어보기

내 기억상 아래와 같은 절차로 했다.  
1. 최신 `origin/develop` 브랜치로 checkout 후, 작업할 브랜치를 만들었다.  
2. commit을 여러 개 추가하고, VSCode의 push 버튼을 눌렀다.
3. 눌렀을 때 팝업이 하나 나왔는데, 뭔지 기억이 잘 안난다.. (가끔 떴을 때 별 문제 아니여서.. 그냥 확인 누름 <- 이게 문제였을 것 같다.)
4. push가 됐으니 PR을 올리려고 하는데 PR 올릴게 없었다.
5. 확인해보니 develop 브랜치에 내 커밋이력이 이미 merge 되어있었다..

push 명령어에 대해 찾아보니 develop 브랜치에 `fix/blabla`가 Fast Forward 되는 형태로 반영된다고 한다.(하필 그 후에 develop에 머지가 된 적이 없었네..)  
근데 PR이 없으면 push 하지 못하도록 설정해놨는데 왜..?

### 로그

Git의 `reflog`에 이런 이력이 있었다.

```bash
[commit id] (HEAD -> fix/blabla) refs/remotes/origin/develop@{10}: update by push
```

다른 push한 이력들은 없었고, 이 브랜치의 push 이력만 있었다.  
뭔가 기존과 다르니까 이력이 이렇게 남은 것 같다..

### 테스트

내 기억상의 절차대로 재연을 해봤다.

1. 최신 `origin/develop` 브랜치로 checkout 후, 작업할 브랜치를 만들었다.  
2. commit을 추가하고, VSCode의 push 버튼을 눌렀다. (팝업 안 나옴)
  ㄴ 이 때 명령어는 `git push -u origin test/1` 이다.
  ㄴ 신규 브랜치를 최초 push 해서 그런거같은데, 그렇다는건 나는 이미 브랜치를 push를 한 적이 있던걸까?
3. 다시 commit을 하고 push 했다.
  ㄴ 이 때 명령어는 `git push origin test/1:test/1` 이다.

재현은 되지 않았지만, VSCode를 통해서 Git을 사용할 때 `push` 하는 방식이 차이가 있다는 점을 알게됐다.  


## 대책
### develop 브랜치 보호

![](https://velog.velcdn.com/images/gingaminga/post/2ee3526f-e8e8-4088-8751-68b71d92bcb7/image.png)

로그에서도 `Bypassed rule violations for refs/heads/develop`라는 문구가 있었는데, 내가 레포지토리의 `Admin`인 상황이라서 그냥 push가 가능했던걸로 보인다.  
이 체크가 해제되어있어 이런 문제가 발생했다고 생각한다.  
하지만 여태까지는 암묵적으로 모든 인원이 PR을 올리면서 작업했기 때문에 크게 문제가 없었다.ㅎㅎ  (만든지 2달도 안 된 레포지토리)  
추후 동일한 상황을 방지하기위해 활성화를 해놨다!
