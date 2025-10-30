---
title: "[Blog] 네이버에 내 블로그 노출시키기 (with Jekyll Theme Chirpy)"
categories: [Blog]
render_with_liquid: false
---

예전에 [구글에 내 블로그 노출시키기](/posts/expose-blog-to-google-with-jekyll-theme-chirpy) 글을 썼었다.  
근데 아직도 구글에서 내 블로그 글 검색이 안된다. 아직도 Sitemap을 가져올 수 없다고 한다.. 하..

그래서 네이버도 설정해보려고 한다.

## Naver Search Advisor
Naver에도 웹사이트의 검색을 모니터링하고 개선할 수 있다.  
[Search Advisor](https://searchadvisor.naver.com/console/board)에서 내 블로그 URL을 등록한다.

![](https://velog.velcdn.com/images/gingaminga/post/0d4a3e62-c30d-407e-9dc2-816855aa0238/image.png)

`사이트 등록` 란이 있는데, 여기에 내 블로그 주소를 적고 버튼을 누른다.

### 소유권 확인
그럼 이 블로그 주소의 소유권을 확인해야하는데 방법은 2가지다.  
- HTML 파일
- HTML 태그

이 중에서 `HTML 태그`로 소유권 확인을 진행할거다.

구글은 `_config.yml`에서 content_id를 추가만 해주면 됐지만, 네이버는 지원하지 않아 직접 태그를 넣어줘야한다.  
그래서 제공받은 태그를 복사해서 `_includes/head.html`에 넣을거다.

```html
  <!-- 생략 -->
  {{ seo_tags }}
  
  <!-- 👇 아래 추가 --> 
  <!-- 네이버 서치 어드바이저 메타태그 추가 --> 
  <meta name="naver-site-verification" content="[content_id]" />

  <title>
    {%- unless page.layout == 'home' -%}
      {{ page.title | append: ' | ' }}
    {%- endunless -%}
    {{ site.title }}
  </title>
```

이제 git에 올려서 배포한다.  

![](https://velog.velcdn.com/images/gingaminga/post/68e5aa98-23be-4434-be92-ece5def32428/image.png)

이제 소유권을 확인해보면 다음과 같이 내 주소에 링크가 걸린걸 볼 수 있다!

### sitemap.xml
이제 sitemap.xml을 제출해야한다.  
이걸 제출해야 검색엔진봇이 사이트 구조를 파악하면서 크롤링을 할 수 있게 된다.  

https://gingaminga.github.io/sitemap.xml 처럼 접근하면 쉽게 존재 여부를 확인할 수 있다.  
`요청 > 사이트맵 제출` 메뉴에서 제출이 가능하다.

![](https://velog.velcdn.com/images/gingaminga/post/398a5a55-ad43-44b1-b607-f48252ba006b/image.png)

> 네이버도 시간이 조금(많이) 걸린다고 한다.. 기다림의 시간.. 
{: .prompt-warning }

---

출처
: - [Github 블로그 Naver 검색 엔진에 노출하기](https://jaehee-kim24.github.io/posts/github%EB%B8%94%EB%A1%9C%EA%B7%B8_%EA%B2%80%EC%83%89%EB%85%B8%EC%B6%9C%ED%95%98%EA%B8%B0_naver/#2-%EC%82%AC%EC%9D%B4%ED%8A%B8%EB%A7%B5-%EC%A0%9C%EC%B6%9C%ED%95%98%EA%B8%B0)