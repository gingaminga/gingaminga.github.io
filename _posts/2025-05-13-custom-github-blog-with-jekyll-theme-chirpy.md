---
title: "[Blog] 내 마음대로 깃허브 블로그 꾸미기 (with Jekyll Theme Chirpy)"
categories: [Blog]
render_with_liquid: false
---

보다보니 변경하고 싶은 것들이 생겼다.  
파비콘부터 시작해서.. 사이드바 하단의 소셜 아이콘 등등..

## 파비콘 변경하기

[Jekyll Theme Chirpy 파비콘 변경 공식 문서](https://chirpy.cotes.page/posts/customize-the-favicon/)를 참고해서 파비콘을 바꿨다.  
근데 나는 마땅한 파비콘 이미지가 없어서.. [다른 파비콘 사이트](https://favicon.io/)에서 쉽게 만들었다.

`assets/img/favicons`에서 `site.webmanifest`은 제외하고 남은 파일은 사이즈별로 파일명까지 매칭하면 끝!
> 없는 파일들은 512 사이즈로 다른 사이트들에서 변경했어요 :)

## 사이드바 메뉴에서 아이콘 제거하기

메뉴에 있는 아이콘이 없었으면 좋겠다는 생각이 들었다.  
`includes/sidebar.html`에서 아이콘을 보여주는 부분을 주석 or 삭제한다.

```html
  <nav class="flex-column flex-grow-1 w-100 ps-0">
    <ul class="nav">
      <!-- home -->
      <li class="nav-item{% if page.layout == 'home' %}{{ " active" }}{% endif %}">
        <a href="{{ '/' | relative_url }}" class="nav-link">
          <i class="fa-fw fas fa-home"></i> {/* 👈 주석 또는 삭제 */}
          <span>{{ site.data.locales[include.lang].tabs.home | upcase }}</span>
        </a>
      </li>
      <!-- the real tabs -->
      {% for tab in site.tabs %}
        <li class="nav-item{% if tab.url == page.url %}{{ " active" }}{% endif %}">
          <a href="{{ tab.url | relative_url }}" class="nav-link">
            <i class="fa-fw {{ tab.icon }}"></i> {/* 👈 주석 또는 삭제 */}
            {% capture tab_name %}{{ tab.url | split: '/' }}{% endcapture %}

            <span>{{ site.data.locales[include.lang].tabs[tab_name] | default: tab.title | upcase }}</span>
          </a>
        </li>
        <!-- .nav-item -->
      {% endfor %}
    </ul>
  </nav>
```

## 사이드바 배경 이미지 넣기

기본적으로 테마색상이 들어가는데, 조금 밋밋해 보여 이미지를 넣고 싶었다.

일단 `assets/img` 하위에 배경화면으로 쓸 파일을 넣는다.  
가로가 `300px`이면 좋긴한데, 이미지가 짤려도 괜찮으면 더 큰거 써도 된다.(~~저는 짤려요 ㅎㅎ~~)

그리고 `_sass/layout/_sidebar.scss`에서 아래처럼 변경하면 끝!

```scss
#sidebar {
  // background: var(--sidebar-bg); /* 👈 기존 내용 주석 */
  background-image: url('/assets/img/sidebar-bg.jpg'); /* 이미지 경로 */
  background-size: cover; /* 이미지 꽉차게 설정하기 */
  background-repeat: no-repeat; /* 이미지 크기가 안 맞는 경우에 이미지가 반복되게 나오지 않게 하기 */
}
```

## 사이드바 하단의 버튼 노출 여부

나는 트위터나 RSS는 딱히 필요없다는 생각이 들었다.  
확인해보니 `_data/contact.yml`의 정보를 보여주고 있었다.  
필요없는건 주석처리하면 된다.  

> 혹시 추가하고 싶은 주소가 있으면, 아이콘은 [Font Awesome](https://fontawesome.com/search)에서 이용하면 된다.

## 글 공유하기에서 버튼 노출 여부

마찬가지로 글을 공유하는 기능에서 URL copy 기능만 있으면 좋겠다.  
`_data/share.yml`에서 보여주고 있어서, 필요없는건 주석처리했다.

## Chirpy theme를 사용한다는 문구 제거

블로그 제일 하단에 `Using the Chirpy theme for Jekyll.`라는 문구가 있는데 없애고싶다.  
근데 프로젝트에서 아무리 검색해도 안 나와 찾아보니, [Footer 수정하기](https://www.irgroup.org/posts/Chirpy-%ED%85%8C%EB%A7%88-%EC%BB%A4%EC%8A%A4%ED%84%B0%EB%A7%88%EC%9D%B4%EC%A7%95/#footer-%EC%88%98%EC%A0%95-%ED%95%98%EA%B8%B0)에서처럼 노출부분을 주석하면 된다.

`_inclues/footer.html`에서 보여주는 영역을 주석 or 삭제한다.  

```html
  <p> {/* 👈 주석 또는 삭제 */}
    {%- capture _platform -%}
      <a href="https://jekyllrb.com" target="_blank" rel="noopener">Jekyll</a>
    {%- endcapture -%}

    {%- capture _theme -%}
      <a
        data-bs-toggle="tooltip"
        data-bs-placement="top"
        title="v{{ theme.version }}"
        href="https://github.com/cotes2020/jekyll-theme-chirpy"
        target="_blank"
        rel="noopener"
      >Chirpy</a>
    {%- endcapture -%}

    {{ site.data.locales[include.lang].meta | replace: ':PLATFORM', _platform | replace: ':THEME', _theme }}
  </p>
```

## 링크를 새 탭으로 열리게 하기

링크를 걸어두면 현재 페이지에서 로드돼서 이탈이 생긴다. 불편하다. 새 탭으로 띄우고 싶다.  
찾아보니 [플러그인을 설치해서 외부 링크를 새 탭](https://jason9288.github.io/posts/github_blog_4/#%EC%99%B8%EB%B6%80-%EB%A7%81%ED%81%AC-%EC%83%88-%ED%83%AD%EC%97%90%EC%84%9C-%EC%97%B4%EB%A6%AC%EA%B2%8C-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)으로 띄울 수 있다.  

`Gemfile` 하단에 `jekyll-target-blank`를 추가한다.
```ruby
# Gemfile

# 링크가 새 탭으로 열리게 하기 
gem 'jekyll-target-blank'
```

그리고 `_config.yml`에도 하단에 추가한다.
```yml
plugins:
  - jekyll-target-blank
```

이제 플러그인을 설치한다.

```bash
# 플러그인 설치
$ bundle install
```

로컬에서 확인해보면 새 탭으로 잘 열린다 :)