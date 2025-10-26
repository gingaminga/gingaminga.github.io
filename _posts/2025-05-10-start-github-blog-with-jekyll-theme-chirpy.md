---
title: "[Blog] Jekyll Theme Chirpy로 깃허브 블로그 시작하기"
categories: [Blog]
---

원래는 velog에서 글을 쓰다가, 단순히 _'나도 광고 넣어볼까?'_ 해서 티스토리 넘어갔다.  
하지만.. 여태 마크다운으로 글을 쓰다보니, 티스토리에서 글쓰기가 싫어졌다. (~~마크다운 지원이 더 잘 됐더라면..)~~  
이것저것 찾아보다가, 마크다운도 잘 지원하면서 이것저것 할 수 있는 **깃허브 페이지**로 다시 옮기기로 했다!
> 나는야 블로그 유목민..😂

# 개발환경 만들기
## Jekyll
마크업 언어(HTML, Markdown 등)로 글을 작성하면 정적 웹사이트를 만들어주는 프레임워크다.  
GitHub에서 Jekyll CMS(Contents Management System)을 내장하고 있어서 호스팅이 쉽게 가능하다.

### 테마 선택
[Jekyll Theme Chripy](https://github.com/cotes2020/jekyll-theme-chirpy)와 [DotX](https://github.com/nandomoreirame/dotX) 중에 고민했는데, 아무래도 사용하는 사람들이 많아야 관련 자료도 금방 찾을 수 있을거라 생각해 <u>Jekyll Theme Chripy</u>로 시작했다.

## 설치
> 자세한 설명은 [Document](https://chirpy.cotes.page/posts/getting-started/#option-2-forking-the-theme)를 참고하세요.

Jekyll Theme Chripy 레포지토리를 [Fork](https://github.com/cotes2020/jekyll-theme-chirpy/fork) 해서 블로그를 만들거다.  
![](https://velog.velcdn.com/images/gingaminga/post/8ab9cccc-950e-4f02-95ec-08dc3f7733be/image.png)

이 때, 레포지토리명은 꼭 `[nickname].github.io` 이어야 한다. (개인 도메인이 있다면 써도 되지만 방법은 찾아보셔야해요 :D)

생성 완료했다된 레포지토리를 내 로컬에 설치하고 IDE로 오픈하면 1차 관문 끝!

```bash
# 주소는 여러분껄로..
$ git clone https://github.com/[nickname]/[nickname].github.io.git
# vscode로 오픈한다면..
$ code ./[nickname].github.io
```

### Ruby
`Jekyll`은 `Ruby`라는 언어로 만들어졌다.  
Mac에는 기본 설치되어있지만 버전이 낮을거라, 최신 Jekyll와의 호환을 위해 안정화 버전을 사용하는 것이 좋다.  
`rbenv`라는 ruby 전용 버전 관리 툴을 설치해서 관리할거다.

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
# 또는 쉘을 껐다 켜도 돼요.
$ source ~/.zshrc
```

이제 rbenv로 ruby를 설치하고 사용한다. (나는 `v3.4.3` 설치)

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
node도 rbenv 처럼 `nvm`이라는 버전 관리 툴을 제공한다.

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
# 또는 쉘을 껐다 켜도 돼요.
$ source ~/.zshrc
```

이제 nvm으로 node를 설치하고 사용한다.

```bash
# node lts 버전 설치
$ nvm install --lts

# node 특정 버전 설치
$ nvm install 20.17.0

# node 설치된 버전들 확인
$ nvm ls

# 설치한 버전 활성화
$ nvm use 20.17.0
```

## 초기화
이제 초기환경으로만 만들어주면 된다.  
친절하게도 초기화해주는 기능을 제공해준다.

```bash
# 기본 제공해주는 초기화 쉘 실행하기
$ ./tools/init.sh
```

마지막에 `Initialization successful!` 이 나오면 성공이다.

## 실행
이제 내 PC에서 블로그를 모습을 확인해 볼 수 있다.

```bash
# 프로젝트 실행
$ bundle exec jekyll serve
```

![](https://velog.velcdn.com/images/gingaminga/post/73115594-2756-423b-be12-d4e9abe54d3f/image.png)

[http://127.0.0.1:4000](http://127.0.0.1:4000) 로 접속하면 내 PC에서만 확인할 수 있는 블로그가 보인다!

> 💡 만약 포트를 변경하고 싶다면 아래처럼 실행해요.
> 
> ```bash
> # 포트를 변경해서 프로젝트 실행
> $ bundle exec jekyll serve -P 4001
> ```
> 
> 그러면 [http://127.0.0.1:4001](http://127.0.0.1:4001)로 접속할 수 있어요.
>

## 설정
이제 `_config.yml` 파일에 필요한 정보들을 넣어 나만의 블로그로 만들어보자.
> 제가 수정한 내용들은 [여기서](https://github.com/gingaminga/gingaminga.github.io/commit/4e8a4a2e11fdf997ab090bdba13b98fc11cf29b1) 확인해보세요ㅎㅎ

- `lang`: 언어 코드
- `timezone`: 시간
- `title`: 블로그 제목. 사이드바의 상단 이미지 밑에 표시됨.
- `tagline`: title 밑에 들어가는 설명 문구
- `description`: SEO 사용 시에 들어가는 문구인데, 나중에 블로그를 검색했을 때의 내용이 들어가야 함.
- `url`: 블로그 주소
- `github.username`: 깃허브 nickname
- `social`: 사이드바의 하단에 있는 아이콘들과 연결됨.
- `avatar`: 사이드바의 상단 이미지 경로
- `paginate`: 한 페이지 개수 설정

# 글쓰기
글은 `_posts` 폴더 하위에 파일을 만들어야하고, 마크다운 문법으로 작성해야한다.

## 파일 규칙
파일명은 `yyyy-mm-dd-[제목].md`으로 작성해야한다.  
만약 제목에 공백이 있으면, `-`로 대체한다.

`2025-05-09-first-post.md` 파일을 만들고 확인해보면 파일명을 기준으로 글이 생성됐다.

![](https://velog.velcdn.com/images/gingaminga/post/aab4476c-a3f4-4557-abe9-5701e81fd536/image.png)

## 파일 내용
마크다운 문법으로 글을 작성한다.  

파일명으로 제목과 날짜를 설정하는데, 이걸로는 뭔가 부족할 수 있다.  
Jeklly에서는 메타 정보를 작성해서 반영되게 하는 `Front Matter` 기능이 있다.  
메타 정보가 기입하면 파일명보다 우선순위가 더 높다.  

```text
---
title: 첫번째 포스트!!
date: 2025-05-05
categories: [개발, Jeklly]
tags: [처음글]
---
```

![](https://velog.velcdn.com/images/gingaminga/post/92e6b24c-ec98-4540-9214-a2cd40e9e2b4/image.png)

메타 정보에 대한 더 자세한 내용은 [Writing a New Post](https://chirpy.cotes.page/posts/write-a-new-post/)에서 확인할 수 있다.

# 배포
## GitHub 설정
배포 전에 GitHub의 빌드 설정을 해야한다.
![](https://velog.velcdn.com/images/gingaminga/post/8a602124-0e66-4c1c-80c9-c9ea64d4e364/image.png)

레포지토리에서 `Settings > Pages > Build and deployment`에서 `source`를 `GitHub Actions`로 선택한다.  

## 업로드
배포는 아주 간단한데, `master` or `main` 브랜치로 작업한걸 올리면 된다.

```bash
# 변경된 사항 전부 스테이징에 올리기
$ git add .
# 커밋 메시지는 원하는거 적어주세요.
$ git commit -m "first commit"
# 원격지에 올리기
$ git push origin master
```

> 💡 만약 commit이 안 된다면?
> 
> 현재 이 프로젝트에서 `husky`를 사용 중이고, `commitlint`를 통해 커밋 시 컨벤션이 설정되어 있어요.(package.json)  
> 나는 혼자 블로그 글을 쓸거라 불필요하다고 판단했고, 그래서 `.husky/commit-msg`를 삭제했어요!

> 💡 만약 push가 안 된다면?
> 
> `./tools/init.sh`을 수행하는 과정에서, (이유는 모르겠으나) 이전 커밋으로 이동한 후에 작업을 진행해요.  
> 그래서 원격지와 충돌이 나는 상황인데,, 저는 원격지와 merge 하고 push 했어요..ㅎㅎ
> ```bash
> # 원격지와 동기화하기
> $ git merge origin/master
> # 원격지에 올리기
> $ git push origin master
> ```

올리면 자동으로 감지돼서 GitHub Actions CI가 실행된다.  
GitHub에서 실시간 상황을 확인할 수 있다.

![](https://velog.velcdn.com/images/gingaminga/post/e7ea50cd-95b0-49bc-9b50-154911f8d8b7/image.png)

완료가 돼도 반영하는데 시간이 조금 걸리는데, 대략 5분 후에 자신의 블로그 주소로 들어가서 확인해보자!