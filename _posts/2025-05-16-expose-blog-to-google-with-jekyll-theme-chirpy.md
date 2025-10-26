---
title: "[Blog] 구글에 내 블로그 노출시키기 (with Jekyll Theme Chirpy)"
categories: [Blog]
---

이왕 글쓴 것들이 공유가 돼서 다른 사람들이 보고 도움을 받거나 피드백을 내가 받아도 좋겠다.  
가만히 있으면 구글 크롤러가 알아서 긁어가줬으면 좋겠지만,,😂 등록을 해서 내 글을 잘 노출시키게 만들어줘야겠다.

## Google Search Console
Google에서는 웹사이트의 검색을 모니터링하고 개선하는데 도움을 주는 무료 툴이다.  
여기에 내 블로그 URL을 등록한다.

![](https://velog.velcdn.com/images/gingaminga/post/814d48e5-b0fc-433f-b025-4d3478b60a1d/image.png)

`URL 접두어`에 내 블로그 주소를 적고 버튼을 누른다.

### 소유권 확인
그럼 이 블로그 주소의 소유권을 확인해야하는데 방법은 여러가지이다.  
- HTML 파일
- HTML 태그
- Google 애널리틱스
- Google 태그 관리자
- 도메인 이름 공급업체

이 중에서 `HTML 태그`로 소유권 확인을 진행할거다.
> 만약 이미 구글 애널리틱스를 만들어둔게 있다면 자동으로 소유권인증이 됐을거다.(내가 그랬다.)
{: .prompt-tip }

HTML 태그를 누르면 meta 정보를 제공받는다.
```html
<meta name="google-site-verification" content="[content_id]" />
```
우리는 여기서 `content_id` 부분이 필요하다. 이 부분만 복사한다.

```yml
webmaster_verifications:
  google: [content_id] # fill in your Google verification code
```
`_config.yml`에 보면 `webmaster_verifications`가 있는데, 거기에 `google`란에 자신의 content_id를 적는다.  

이제 git에 올려서 배포한다.  
배포가 완료되면 다시 소유권 확인을 해본다.

![](https://velog.velcdn.com/images/gingaminga/post/84ba069c-cc51-4839-970b-54cbb8f644cb/image.png)

나는 확인 방법이 Google 애널리틱스라고 나오지만, 위에서 설명한대로 했다면 `HTML 태그`라고 나올 것이다.  
`속성으로 이동` 버튼을 클릭해서 이동하면 절반 성공!

### sitemap.xml
이제 sitemap.xml을 제출해야한다.  
이걸 제출해야 검색엔진봇이 사이트 구조를 파악하면서 크롤링을 할 수 있게 된다.  
GitHub Pages는 배포하면 기본적으로 제공해준다. 너무 친절하다.ㅎㅎ

https://gingaminga.github.io/sitemap.xml 처럼 접근하면 쉽게 존재 여부를 확인할 수 있다.

![](https://velog.velcdn.com/images/gingaminga/post/776ea252-f7f3-4b86-ae89-058067b505c0/image.png)

`Sitemaps` 메뉴에 들어가 사이트맵을 추가하면 제출 완료다.
> 상태가 '가져올 수 없음'인데, '성공'이라고 노출되어야 한다. 시간이 조금 걸린다.(많이..) 
{: .prompt-warning }

---

출처
: - [Github 블로그 Google 검색 엔진에 노출하기](https://jaehee-kim24.github.io/posts/github%EB%B8%94%EB%A1%9C%EA%B7%B8_%EA%B2%80%EC%83%89%EB%85%B8%EC%B6%9C%ED%95%98%EA%B8%B0)
: - [Jekyll 테마에서 구글 검색 노출 등록하기](https://www.irgroup.org/posts/jekyll-google-search/)
: - [sitemap을 읽어올 수 없다는 에러 발생 문의](https://support.google.com/webmasters/thread/339589413/jekyll-chirpy-%ED%85%8C%EB%A7%88-github-io%EC%9D%98-sitemap-xml%EC%9D%84-%EC%9D%BD%EC%96%B4%EC%98%AC-%EC%88%98-%EC%97%86%EB%8B%A4%EB%8A%94-%EC%97%90%EB%9F%AC%EA%B0%80-%EB%B0%9C%EC%83%9D%ED%95%A9%EB%8B%88%EB%8B%A4?hl=ko)