---
title: "[GitHub] PR 자동으로 만들기"
categories: [GitHub]
render_with_liquid: false
---

나는 혼자 개발할 때가 많다.  
그럴때마다 PR을 만들고.. 셀프 리뷰하고.. 머지하는 작업은 사실 쉽지않다.  
그렇다고 PR을 안 만들기엔 이력 관리가 잘 안 된다.  
일단 PR이라도 자동으로 만들어봐야겠다.

> GitHub에 프로젝트가 있고, Jira와 연동되어있다고 가정합니다.  
> Jira를 사용하지 않더라도, 관련 내용은 빼고 보셔도 무방합니다.

## GitHub Actions
GitHub을 사용하고 있으니, 추가적인 설치가 필요없는 `GitHub Actions`를 사용한다.  
깃헙짱..

### workflow
GitHub Actions에서는 하나의 자동화된 전체 프로세스를 `workflow`라고 부른다.  
예를들어 어떠한 이벤트(push, pr 등)가 발생하면 주어진 일(PR 생성 등)을 실행하는 일련의 프로세스를 뜻한다.

workflow는 `yml` 파일로 작성한다.  
크게 `job` 단위로 작업을 구분하고, 각 job은 `step` 단위를 통해 단계별로 실행되도록 한다.  
job은 여러 개 있을 수 있으며, 여러 job이 독립적으로 실행될 수도 있고, 다른 job끼리 의존되게 실행될 수도 있다.

자세한 내용은 [Github Action의 코어 개념](https://velog.io/@ggong/Github-Action%EC%97%90-%EB%8C%80%ED%95%9C-%EC%86%8C%EA%B0%9C%EC%99%80-%EC%82%AC%EC%9A%A9%EB%B2%95#github-action%EC%9D%98-%EC%BD%94%EC%96%B4-%EA%B0%9C%EB%85%90)을 참고하면 좋을 것 같다.ㅎㅎ

### 자동 PR 만들기
내가 사용하고 있는 `자동 PR workflow`를 공유한다.  
프로젝트 root에 `.github/workflows/auto-pr.yml`에 만든다.

```yml
name: Auto PR

permissions:
  contents: read
  pull-requests: write
  issues: write

on:
  push:
    branches:
      - "feature/ITS-*"

jobs:
  auto-pr:
    runs-on: ubuntu-latest
    steps:
      - name: 체크아웃
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: 브랜치명에서 지라 이슈키 추출
        run: |
          branch="${GITHUB_REF#refs/heads/}"
          echo "branch=$branch" >> $GITHUB_ENV
          if [[ "$branch" =~ ([A-Z]+-[0-9]+) ]]; then
            echo "issue_key=${BASH_REMATCH[1]}" >> $GITHUB_ENV
          else
            echo "issue_key=" >> $GITHUB_ENV
          fi

      - name: 지라 이슈 제목 가져오기
        if: env.issue_key != ''
        run: |
          jira_response=$(curl -s -u "${{ secrets.JIRA_EMAIL }}:${{ secrets.JIRA_API_TOKEN }}" \
            -H "Accept: application/json" \
            "https://gingaminga.atlassian.net/rest/api/3/issue/${{ env.issue_key }}")
          title=$(echo "$jira_response" | jq -r '.fields.summary')
          echo "jira_title=$title" >> $GITHUB_ENV

      - name: PR 생성하기
        uses: thomaseizinger/create-pull-request@master
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          head: ${{ github.ref_name }}
          base: develop
          title: "[${{ env.issue_key }}] ${{ env.jira_title }}"
          body: |
            자동 생성된 PR입니다.
            - 지라이슈: [${{ env.issue_key }}](https://gingaminga.atlassian.net/browse/${{ env.issue_key }})

      - name: PR에 Assignee 추가하기
        uses: actions/github-script@v7
        with:
          script: |
            const { repo, owner } = context.repo;
            const prList = await github.rest.pulls.list({
              owner,
              repo,
              head: `${owner}:${process.env.branch}`,
              state: 'open'
            });

            if (prList.data.length > 0) {
              const prNumber = prList.data[0].number;

              // Assignee 추가
              await github.rest.issues.addAssignees({
                owner,
                repo,
                issue_number: prNumber,
                assignees: [owner]
              });

              console.log(`Assignee를 ${owner}로 추가 완료 :)`);
            } else {
              console.log("PR을 찾지 못했습니다. :(");
            }
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          branch: ${{ github.ref_name }}
```

흐름은 다음과 같다.
1. feature/[이슈키] 브랜치에서 push 이벤트 발생 시 workflow 동작
2. 브랜치명에서 **이슈키** 추출
3. 지라 이슈 제목 가져오기 (추후 PR의 제목이 될 것)
4. PR 생성
5. 생성된 PR에 `assignee` 추가

#### GITHUB_TOKEN
workflow 내용 중간중간 `secrets.GITHUB_TOKEN`을 사용한다.  
PR을 만들고 수정하는 작업은 아무나 할 수 없기 때문에, 미리 토큰을 만들고 사용할 수 있도록 한다.

[개인 토큰 만들기](https://github.com/settings/personal-access-tokens)에서 만들 수 있다.  
만들 때 하단에 `Permissions`가 있는데, 그 중 **Repository Permissions**에서 몇 가지 선택을 해야한다.  

![](https://velog.velcdn.com/images/gingaminga/post/1fbb4ab5-aa2f-4371-b982-74fd66f2bb8d/image.png)
레포지토리의 내용(commit, branch 등)을 읽을 권한을 준다.

![](https://velog.velcdn.com/images/gingaminga/post/f68cdbaf-a8da-4b0b-8ddd-3c7c3f601bed/image.png)
PR에 관련된 내용(label, assignee 등)을 읽고 쓰는 권한을 준다.

그 후 생성을 하면 토큰이 만들어진다.  
토큰값은 더 이상 확인이 안 되니 어딘가에 꼭 저장을 해둔다.  

이제 레포지토리로 이동 후, `Settings > Secret and variables > Actions`에서 **Repository secret**에 토큰을 등록한다.

![](https://velog.velcdn.com/images/gingaminga/post/b9b36171-c027-435c-ad31-01a038f9fbd6/image.png)

이름이 꼭 `secret.GITHUB_TOKEN`일 필요는 없다. 하지만 이름을 바꾸면 workflow에서도 바꿔줘야 한다는걸 잊지말자.

#### JIRA_EMAIL, JIRA_API_TOKEN

`JIRA_EMAIL`과 `JIRA_API_TOKEN`는 jira 이슈키를 통해 **지라의 타이틀명**을 가져오기 위해 사용한다.

`JIRA_EMAIL`은 jira 가입 시 등록한 이메일을 사용하면 된다.  
`JIRA_API_TOKEN`은 jira에서 발급받아 사용하면 된다. [지라 토큰 생성하기](https://id.atlassian.com/manage-profile/security/api-tokens)에서 금방 만들 수 있다.

이 두 개 또한 **Repository secret**에 등록하면 끝이다.

### PR 생성

이제 이 워크플로우를 commit & push 하면 자동으로 PR이 생성된다.

![](https://velog.velcdn.com/images/gingaminga/post/9788e93c-61ba-433f-8ac4-ac3bcb16321e/image.png)

이제 생성해야하는 귀찮음이 사라졌다!!

이제 리뷰도 좀 자동으로 해주면 좋을텐데..  
ㄴ [리뷰 자동으로 하는 방법 보러가기](/posts/ai-automatically-pr-review)