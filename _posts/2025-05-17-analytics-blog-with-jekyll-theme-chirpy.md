---
title: "[Blog] 내 블로그 통계 보기 (with Jekyll Theme Chirpy)"
categories: [Blog]
---

내 글에 대한 통계를 확인하고 싶다!🔥🔥  
Google Analytics에 등록하면 **무료로** 가능하다고 해 연동을 해보겠다.

## Google Analytics 생성
[Google Analytics](https://analytics.google.com/analytics/web)에 들어가면 만들기 > 계정을 선택한다.

계정은 <u>관리의 단위</u>라고 생각해서 내 닉네임(`gingaminga`)를 적었다. (체크하는건 다 default..)  
속성은 <u>개인이 관리하는 여러 프로젝트</u>라고 생각했고, 구분을 쉽게 하기 위해서 블로그 주소(`gingaminga.github.io`)를 적었다.  
비지니스 세부정보는 `컴퓨터 및 전자제품`과 `작음`을 선택했다.  
비지니스 목표는 `리드 생성`, `웹 또는 앱 트래픽 파악`, `사용자 참여 발생 시간 및 유지율 보기`를 선택했다.  
약관 동의 후 플랫폼은 `웹`을 선택했다. 그 후 URL과 스트림을 입력한다.
![](https://velog.velcdn.com/images/gingaminga/post/49d9142a-4459-49f2-aa5d-87fa5a4a7e7f/image.png)

## 측정ID 등록
생성을 완료하면 `측정 ID`라는 값이 확인 가능하다.  
그 값을 `_config.yml`의 `analytics`에 넣어준다.

```yml
analytics:
  google:
    id: [측정ID] # fill in your Google Analytics ID
```

이제 배포하고나서, 블로그에 접속해본다.  
그 후에 애널리틱스로 가서 카운팅이 됐는지 확인해본다.

![](https://velog.velcdn.com/images/gingaminga/post/ad07b7d3-6bd2-421c-9c41-2ceb5e6a38ae/image.png){: w="500" h="500"}

카운팅이 되면 성공!!

## Google Search Console 연동
만약 구글 서치콘솔이 있다면, 애널리틱스랑 연동해두면 좋다.  
검색 노출로 인한 접근 등에 대한 통계도 받을 수 있다.  
[Google Search Console 등록하는 방법](/posts/analytics-blog-with-jekyll-theme-chirpy)에 대한 글도 공유한다.ㅎㅎ

### 연동방법
Google Analytics 왼쪽 하단에 보면 '관리' 버튼이 있다.  
제품 링크 > Search Console 링크 > 연결 을 선택한다.  

그 후 `Search Console`과 `웹 스트림`을 선택하고 연동을 시키면 끝!(연동하는건 그냥 선택만 하면 된다.)

---

출처
: - [Chirpy 테마로 GitHub 블로그 만들기 ④](https://jason9288.github.io/posts/github_blog_4/#%EA%B5%AC%EA%B8%80-%EC%95%A0%EB%84%90%EB%A6%AC%ED%8B%B1%EC%8A%A4-%EB%93%B1%EB%A1%9D)
: - [Github.io 블로그에 Google Analytics 적용하기](https://mino1982.tistory.com/39)