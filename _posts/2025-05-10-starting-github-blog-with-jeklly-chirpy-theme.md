---
title: Jekyll Chirpy Theme로 깃허브 블로그 시작하기
---

원래는 velog에서 글을 쓰다가, 단순히 '나도 광고 넣어볼까?' 해서 티스토리 넘어갔다.  
하지만.. 여태 마크다운으로 글을 쓰다보니, 티스토리에서 글쓰기가 싫어졌다. (~~마크다운 지원이 더 잘 됐더라면..)~~

이것저것 찾아보다가, 마크다운도 잘 지원하면서 이것저것 할 수 있는 **깃허브 페이지**로 다시 옮기기로 했다!
> 나는야 블로그 유목민..😂

# 개발환경 만들기
## Jekyll
마크업 언어(HTML, Markdown 등)로 글을 작성하면 정적 웹사이트를 만들어주는 프레임워크다.  
GitHub에서 Jekyll CMS(Contents Management System)을 내장하고 있어서 호스팅이 쉽게 가능하다.

### Jekyll Chirpy Theme
[Jekyll Chirpy Theme](https://github.com/cotes2020/jekyll-theme-chirpy)와 [DotX](https://github.com/nandomoreirame/dotX) 중에 고민했는데, 아무래도 사용하는 분들이 많고 관련 자료를 금방 찾을 수 있는 Jekyll Chirpy Theme로 선택했다.

## 설치
### Repository
설치 방법은 [Docs](https://chirpy.cotes.page/posts/getting-started/#option-2-forking-the-theme)에 설명되어있는데, 그 중 `Fork` 해서 블로그를 만들기로 했다.  
이 때, 레포지토리명은 꼭 `[nickname].github.io` 이어야 한다.  
개인 도메인이 있으면 써도 된다는데, 나는 없기 때문에..ㅎㅎ

![](https://velog.velcdn.com/images/gingaminga/post/8ab9cccc-950e-4f02-95ec-08dc3f7733be/image.png)

생성된 레포지토리를 내 로컬에 `clone` 받는다.
```bash
# 주소는 여러분껄로..
$ git clone https://github.com/[nickname]/[nickname].github.io.git
```

이제 사용하는 IDE로 열면 1차 관문 끝!

### Ruby
`Jekyll`은 `Ruby`라는 언어로 만들어졌다.  
Mac에는 기본 설치되어있지만 버전이 낮을거라, 최신 Jekyll와의 호환을 위해 안정화 버전을 사용하는 것이 좋다.  
그래서 버전 관리 툴인 `rbenv`를 설치해서 관리한다.

```bash
# rbenv 설치
$ brew install rbenv
```

rbenv를 전역에서 사용하도록 환경변수 설정도 한다.

```bash
# 1. 쉘 설정파일 수정
$ vi ~/.zshrc

# 👇 제일 하단에 아래 내용을 추가해주세요.

# ruby
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"

# 다 작성 후 :wq로 저장

# 2. 쉘 설정 바로 적용
# 이거 안할거면 쉘을 껐다 키세요.
$ source ~/.zshrc
```

이제 rbenv로 ruby를 설치하고 사용한다.(나는 `v3.4.3` 설치)

```bash
# ruby 안정화 버전 리스트
$ rbenv install -l

# ruby 특정 버전 설치
$ rbenv install 3.4.3

# 설치한 버전 활성화
$ rbenv global 3.4.3
```

### Node.js
해당 테마는 `Node.js`도 필요하다.  
왜 필요한지 보니까 `javascript` 파일들을 `rollup`으로 번들링 및 이것저것 다른 것들도 하고 있었다.  
node도 rbenv 처럼 `nvm`이라는  버전 관리 툴을 제공한다.

```bash
$ brew install nvm
```

nvm도 전역에서 사용하도록 환경변수 설정도 한다. (설정 방법은 비슷하다.)
```bash
# 1. 쉘 설정파일 수정
$ vi ~/.zshrc

# 👇 제일 하단에 아래 내용을 추가해주세요.

# nvm
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh" # This loads nvm
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

# 다 작성 후 :wq로 저장

# 2. 쉘 설정 바로 적용
# 이거 안할거면 쉘을 껐다 키세요.
$ source ~/.zshrc
```

이제 nvm으로 node를 설치하고 사용한다. (기존에 node를 사용하고 있어서 `v20.17.0`을 사용하지만, 다른 분들인 lts 버전 사용하세요!)

```bash
# node lts 버전 설치
$ nvm install

# node 특정 버전 설치
$ nvm install 20.17.0

# 설치한 버전 활성화
$ nvm use 20.17.0
```
## 초기화
이제 소스코드를 초기화만 시키면 된다.

```bash
# 프로젝트에서 초기화해주는 기능을 제공해준다.
$ ./tools/init.sh
```

마지막에 `Initialization successful!` 이 나오면 성공이다.

## 실행
이제 이 블로그의 생김새를 한 번 확인해보자.
```bash
$ bundle exec jekyll serve
```

그러면 중간에 `http://127.0.0.1:4000`가 보일건데, 그게 자신의 PC에서만 확인할 수 있는 주소다!

![](https://velog.velcdn.com/images/gingaminga/post/73115594-2756-423b-be12-d4e9abe54d3f/image.png)
이 화면이 나오면 성공이다. :)

# 글쓰기
글은 `_posts` 폴더 하위에 파일을 만들어야하고, 마크다운 문법으로 작성해야한다.

## 파일 규칙
파일명은 `yyyy-mm-dd-[제목].md`으로 작성해야한다.  
만약 제목에 공백이 있으면, `-`로 대체한다.

`2025-05-09-first-post.md`로 만들고 확인해보면 바로 반영된다.

![](https://velog.velcdn.com/images/gingaminga/post/aab4476c-a3f4-4557-abe9-5701e81fd536/image.png)

## 파일 내용
기본적으로 마크다운 문법을 사용한다.  
상단에는 메타 정보를 작성할 수 있다. (`Front Matter`라고 불림!)  
날짜와 제목은 파일명에 있지만, 메타 정보가 기입하면 우선순위가 더 높다.

![](https://velog.velcdn.com/images/gingaminga/post/cbfced9d-6dd7-408f-9f48-844f6268aa9e/image.png)

파일 최상단에 아래 내용을 작성하고 확인해본다.
```text
---
title: 첫번째 포스트!!
date: 2025-05-05
categories: [개발, Jeklly]
tags: [처음글]
---
```

![](https://velog.velcdn.com/images/gingaminga/post/eecdda94-96e2-441c-bccd-5ce29ff6e663/image.png)

더 자세한 내용은 [Writing a New Post](https://chirpy.cotes.page/posts/write-a-new-post/)에서 확인할 수 있다.

# 배포
이제 글 쓴걸 실제 주소에 배포해서 확인해보자.

```bash
# 변경된 사항 전부 스테이징에 올리기
$ git add .
# 커밋 메시지는 원하는거 적어주세요.
$ git commit -m "first commit"
# 원격지에 올리기 (아마 main or master 브랜치)
$ git push origin [branch name]
```

main 또는 master 브랜치를 원격지에 올리면 자동으로 GitHub Actions CI가 실행된다.

![](https://velog.velcdn.com/images/gingaminga/post/e7ea50cd-95b0-49bc-9b50-154911f8d8b7/image.png)

만약 에러가 발생한다면.. 레포지토리에서 `Settings > Pages > Build and deployment`에서 `source`를 `GitHub Actions`로 선택한다.  
다시 실패한 `workflow`로 가서 `Re-run jobs` 버튼이 있을텐데, 누르면 빌드를 다시 시작할거다.

완료가 돼도 반영하는데 시간이 조금 걸리는데, 약 5분 뒤에 자신의 블로그로 들어가서 확인해보자!