---
title: "[GitHub] AI가 PR 자동 리뷰하기"
categories: [GitHub]
render_with_liquid: false
---

[PR을 자동으로 만들었으니](/posts/create-pr-automatically), 이 PR을 분석해서 리뷰까지 해주면 좋겠다.  
그래서 PR이 올라오면 자동으로 AI가 리뷰까지 해주도록 설정해보겠다.

## CodiumAI(Qodo)
요즘에는 AI 리뷰 툴이 엄청 많다.  
최대한 무료인걸 찾아봤는데, `pr-agent`가 있었다.

`pr-agent`는 chatGPT 등 LLM을 활용해 리뷰를 할 수 있게 해주는 툴이다.  
결국 LLM을 사용하려면 결제가 필요한데,, `pr-agent`를 가지고 오픈소스로 만들어둔 툴이 [CodiumAI](https://www.qodo.ai/)이다!  
물론 무료니까.. PR 올리는 내용들을 통해 모델 학습에 사용될 수 있으니 참고하셔야합니다.

### 설치
[GitHub App으로 지원](https://github.com/marketplace/codiumai-pr-agent)하니, 여기서 쉽게 설치할 수 있다.  
설치하면 사실 끝이다.. 이제 PR을 올려보자!

### 리뷰 확인하기
![](https://velog.velcdn.com/images/gingaminga/post/325433cd-31fc-43f9-8dff-64fb98230d98/image.png){: width="600" height="400" }

이렇게 리뷰를 만들어준다!!  

해당 PR 코멘트에 `/review`, `/describe`, `/improve` 등의 명령어를 입력하면 재리뷰도 받을 수 있다.  
추가적인 명령어는 [여기서](https://qodo-merge-docs.qodo.ai/tools/) 확인할 수 있다.

### 한국어로 리뷰받기
하다보니 한국어로 리뷰가 나오면 더 좋을 것 같다.(~~영어를 못해서가 아님!!~~)  
설정하는 방법 중 [Local configuration file](https://qodo-merge-docs.qodo.ai/usage-guide/configuration_options/#local-configuration-file) 방법으로 설정했다.

프로젝트 루트에 `.pr_agent.toml`을 만들고 내용을 작성한다.
```toml
[config]
response_language = "ko-KR"
```

근데 계속 적용이 안 된다!!  
찾아보니 `pr-agent`를 사용하기 전에 설정을 미리 업데이트 해놔야했다.(~~난 바보..~~)  
정리하자면.. 부모 브랜치에서 미리 설정을 하고 자식 브랜치를 만들어야 설정한 내용이 적용된다!  

![](https://velog.velcdn.com/images/gingaminga/post/9df678cb-ef50-4e88-9403-06e045c9ae2f/image.png){: width="600" height="400" }
_description_
![](https://velog.velcdn.com/images/gingaminga/post/baa90a4f-e2ac-4682-af41-e1c27a07e38e/image.png){: width="600" height="400" }
_review_

이제 한국어로도 설명이 작성되고, 리뷰도 한국어로 잘 설명해준다 :) (오타도 알려주고 좋다)