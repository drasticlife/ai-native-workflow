---
title: Obsidian 노트를 무료 웹사이트로 발행하기 — GitHub Publish + Quartz 실전 가이드
tags:
  - obsidian
  - quartz
  - github-pages
  - 사내교육
  - 지식공유
created: 2026-07-28
updated: 2026-07-28
---

## 개념과 배경

- **왜 필요한가:** 볼트에 쌓아둔 노트는 나만 본다. 사내에 공유하려면 웹으로 꺼내야 하는데, 공식 Obsidian Publish는 월 구독이다.
- **무엇인가:** `GitHub Publish` 커뮤니티 플러그인이 지정한 폴더의 노트를 GitHub 저장소로 밀어 넣고, GitHub Actions가 **Quartz**(정적 사이트 생성기)로 빌드해 GitHub Pages에 배포한다. 전부 무료다.
- **결과물:** `https://<계정>.github.io/<저장소>` 주소의 사이트. 좌측 폴더 트리, 검색, 다크모드, 우측 목차·백링크·그래프뷰가 기본으로 딸려 온다.
- **걸리는 시간:** 처음 개설까지 약 15분. 이후 발행은 클릭 한 번.

전체 흐름은 이렇게 돌아간다.

![[Pasted image 20260728223634.png]]

내가 하는 건 **볼트에서 노트를 쓰고 "Publish changes"를 누르는 것**까지다. GitHub 저장소로 넘어간 뒤부터는 전부 자동으로 돌아간다.

## 준비물과 도구

- **Obsidian** + 커뮤니티 플러그인 `GitHub Publish` (제작자 oilandrust)
- **GitHub 계정** — 없으면 아래 첫 단계에서 만든다. 저장소는 플러그인이 자동으로 만들어 준다
- **`gh` CLI** (선택) — 커스텀 CSS를 붙일 때만 필요
- **발행할 폴더** — 볼트 안 아무 폴더. 이 폴더의 하위 전체가 사이트가 된다

> [!note]
> 사이트는 **폴더 단위**로 발행된다. 볼트 전체가 아니라 지정한 한 폴더의 하위 트리만 올라간다.
> 그리고 그 폴더 **바로 밑**에 `index.md`가 있어야 사이트 홈페이지가 생긴다.

이 문서의 메뉴 이름은 **영문 UI 기준**이고 괄호에 한글을 함께 적었다. Obsidian 언어 설정과 무관하게 따라갈 수 있다.

## 핵심 규칙

> [!tip] 진입점을 구분할 것
> 리본(좌측 세로 막대)의 **지구본 아이콘**은 "Set up site"(최초 설정)에 연결돼 있다. 누를 때마다 폴더 선택 마법사가 뜨고, 재초기화되면서 손댄 설정이 원복된다.
> 평소 발행은 **Command palette(`Ctrl+P`) → "GitHub Publish: Publish changes"**.

> [!tip] 고친 건 발행해야 반영된다
> 노트를 쓰든 설정을 바꾸든, **"Publish changes"를 눌러야** 사이트에 올라간다.
> 저장하는 것과 발행하는 것은 별개 동작이다.

## 실습

이 문서는 두 덩이다. **먼저 아래 다섯 단계까지만 하면 사이트가 뜬다.** 나머지는 나중에 필요할 때 와도 된다.

| | 단계 | 언제 |
|---|---|---|
| 1단계 — 일단 띄우기 | 계정 만들기 → 플러그인 설치 → 사이트 개설 → 홈페이지 → 발행 | 오늘 |
| 2단계 — 다듬기 | 설정 파일 → 테마 → 컴포넌트 → 커스텀 CSS → 작성 규칙 | 사이트가 뜬 뒤, 마음에 안 드는 게 생겼을 때 |

### GitHub 계정 만들기

이미 계정이 있으면 건너뛴다.

1. 브라우저에서 `github.com` 접속
2. 우측 상단 **Sign up** 클릭
3. 이메일 → 비밀번호 → 사용자 이름(username) 순으로 입력
4. 이메일로 온 인증 코드 입력

> [!warning]
> 여기서 정한 **username이 사이트 주소에 그대로 들어간다.** `https://<username>.github.io/<저장소>` 형태다.
> 사내 공유용이라면 알아보기 쉬운 이름으로 정할 것. 나중에 바꾸면 주소도 바뀐다.

### 플러그인 설치하기

1. Obsidian 좌측 하단 **톱니바퀴 아이콘** 클릭 → Settings(설정) 창이 열린다
2. 왼쪽 목록에서 **Community plugins(커뮤니티 플러그인)** 클릭
3. 처음이면 **Turn on community plugins(커뮤니티 플러그인 사용)** 버튼을 눌러 활성화한다
4. **Browse(탐색)** 버튼 클릭 → 플러그인 검색 창이 열린다
5. 검색창에 `GitHub Publish` 입력
6. 검색 결과에서 **제작자가 `oilandrust`인 항목**을 클릭
7. **Install(설치)** → 설치가 끝나면 **Enable(활성화)**

![[Pasted image 20260728223459.png]]

![[Pasted image 20260728223309.png]]

> [!warning]
> 이름이 비슷한 **`GitHub Sync`** 라는 다른 플러그인이 있다. 그건 볼트 전체를 백업하는 용도라 목적이 다르다.
> **플러그인 이름과 제작자 이름으로 구분**할 것.

### 사이트 개설하기

1. `Ctrl+P` → `Set up site` 입력 → **"GitHub Publish: Set up site"** 선택
2. GitHub 로그인 화면이 뜨면 로그인하고 권한을 승인한다
3. **폴더 선택 화면**이 나온다. 발행할 폴더를 하나 고른다

> [!example] 📸 화면 02 — 폴더 선택 트리
> **찍을 것:** Set up site 마법사의 폴더 트리 화면. 볼트 폴더 목록이 펼쳐져 있고 하나가 선택된 상태.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/02-폴더-선택.png]]` 로 바꾸면 끝.

> [!warning]
> 여기서 고른 폴더가 **사이트 전체 범위**가 된다. 이 폴더 안의 모든 노트가 인터넷에 공개된다.
> 공개해도 되는 노트만 들어 있는 폴더를 고를 것. 나중에 바꾸려면 재설정해야 한다.

4. 다음 화면에서 **저장소 이름**을 입력한다

> [!example] 📸 화면 03 — 저장소 이름 입력
> **찍을 것:** 저장소 이름을 입력하는 화면. 입력란과 다음(생성) 버튼이 보이게.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/03-저장소-이름.png]]` 로 바꾸면 끝.

여기 입력한 이름이 **주소가 된다.** `my-wiki`로 하면 `https://<username>.github.io/my-wiki`.
짧게, 영문 소문자와 하이픈만 쓰는 것이 좋다.

5. 생성 버튼을 누르면 진행 모달이 뜬다

> [!example] 📸 화면 04 — 발행 진행 모달
> **찍을 것:** 발행이 진행되는 모달. "Building and deploying site…" 같은 진행 문구가 보이는 순간.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/04-발행-진행.png]]` 로 바꾸면 끝.

**이 화면이 나오면 정상이다.** 플러그인이 저장소를 만들고 아래를 커밋한다.

- `content/` — 노트 사본
- `quartz.config.yaml` — 사이트 설정
- `.github/workflows/deploy.yml` — 빌드·배포 워크플로우

빌드에 2~10분 걸린다. 완료되면 주소가 안내된다.

### 홈페이지 만들기

여기서 대부분 한 번은 막힌다. 발행 폴더 **바로 밑**에 `index.md`가 없으면 **루트 주소가 404**가 된다.

1. 발행 폴더를 **우클릭** → **New note(새 노트)**
2. 파일명을 `index` 로 지정한다 (확장자 `.md`는 자동으로 붙는다)
3. 아래 내용을 붙여넣고 내용만 바꾼다

```markdown
---
title: 내 사이트 이름
---

사이트 소개 한 줄.

## 목차

- [[첫-번째-노트]]
```

> [!warning]
> 하위 폴더가 아니라 **발행 폴더 바로 밑**이어야 한다.
> `발행폴더/index.md` → 맞음 / `발행폴더/어딘가/index.md` → 홈페이지가 되지 않음

### 노트 발행하기

노트를 쓰거나 고친 뒤 발행하는 절차다. **앞으로 매번 반복할 유일한 작업이다.**

1. `Ctrl+P` (Command palette 열기)
2. `publish` 입력
3. **"GitHub Publish: Publish changes"** 선택

> [!example] 📸 화면 05 — Command palette
> **찍을 것:** `Ctrl+P`로 연 Command palette에 `publish` 입력한 상태. "Publish changes"와 "Set up site"가 **둘 다 보이는** 화면이면 좋다 (구분이 목적).
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/05-command-palette.png]]` 로 바꾸면 끝.

사이트가 하나면 선택창 없이 바로 발행된다. 발행 폴더 하위는 **재귀 스캔**되므로 하위 폴더를 자유롭게 나눠도 된다.

매번 `Ctrl+P`를 치기 번거로우면 단축키를 걸어둔다.

4. Settings(설정) → **Hotkeys(단축키)**
5. 검색창에 `publish` 입력
6. `GitHub Publish: Publish changes` 행 오른쪽의 **`+` 버튼** 클릭
7. 원하는 키 조합을 누른다

> [!example] 📸 화면 06 — 단축키 바인딩
> **찍을 것:** Hotkeys 설정에서 `publish` 검색한 상태. `Publish changes` 행과 `+` 버튼이 보이게.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/06-단축키-바인딩.png]]` 로 바꾸면 끝.

> [!warning]
> `Ctrl+Shift+S`는 `GitHub Sync` 플러그인이 이미 쓰고 있을 수 있다. 겹치면 Obsidian이 경고를 띄운다.

### 설정 파일 찾기

> [!note] 여기부터 2단계 — 사이트가 뜬 뒤에 와도 된다
> 지금까지가 "일단 띄우기"였다. 아래는 외형이 마음에 안 들 때 손보는 내용이다.
> 클릭이 아니라 **파일 편집**이라 조금 더 손이 간다.

사이트 외형을 바꾸려면 `quartz.config.yaml`을 고쳐야 한다.

Obsidian 안에서 여는 방법:

1. Settings(설정) → 왼쪽 목록 아래쪽의 **GitHub Publish**
2. **Published sites** 항목에서 내 사이트 카드를 찾는다
3. 카드의 **"Edit Quartz config"** 버튼 클릭
4. YAML 편집창이 열린다. 고친 뒤 **Save**

> [!example] 📸 화면 07 — 사이트 카드
> **찍을 것:** Settings → GitHub Publish 화면. 사이트 카드와 "Edit Quartz config" 버튼이 보이게.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/07-사이트-카드.png]]` 로 바꾸면 끝.

> [!example] 📸 화면 08 — Quartz config 편집창
> **찍을 것:** "Edit Quartz config"를 눌렀을 때 뜨는 YAML 편집 모달. Save / Reset to default 버튼이 함께 보이게.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/08-config-편집창.png]]` 로 바꾸면 끝.

파일을 직접 열려면 이 경로다.

```
.obsidian/plugins/github-publish/sites/<owner>_<repo>/quartz.config.yaml
```

사이트 ID(`owner/repo`)의 `/`가 `_`로 치환된 폴더명이다. `drasticlife/my-wiki` 라면 `drasticlife_my-wiki` 폴더.

> [!warning]
> **GitHub 웹에서 `quartz.config.yaml`을 고치면 안 된다.** 다음 발행 때 이 로컬 파일 내용으로 덮어써진다.
> 위 핵심 규칙의 파일 소유권 그림을 참고할 것.

**설정을 고친 뒤에는 반드시 "Publish changes"를 한 번 실행해야** 사이트에 반영된다.

### 테마 고르기

`quartz.config.yaml`에서 `quartz-themes` 항목을 찾아 `theme` 값만 바꾸면 된다.

```yaml
  - source:
      name: quartz-themes
      repo: github:saberzero1/quartz-themes
      subdir: plugin
    enabled: true
    options:
      theme: obsidian
```

바꾼 뒤 **Save → Publish changes**. 2~10분 뒤 사이트에 반영된다.

프리셋이 수백 개인데 **이름만 보고 고르면 안 된다.** 예를 들어 `publisher`라는 테마는 Obsidian Publish와 아무 관계 없는 주황색 테마다. 색을 미리 확인하려면 터미널에서:

```bash
gh api repos/saberzero1/quartz-themes/contents/plugin/src/themes/<이름>.json \
  --jq .content | base64 -d | grep -oE '\-\-accent-h: [0-9]+' | head -1
```

`--accent-h`는 색상환 각도다. `258`이면 보라, `11`이면 주황, `210`이면 파랑.

### 컴포넌트 켜고 끄기

화면 요소 하나하나를 켜고 끌 수 있다. `plugins:` 배열에서 `enabled`로 on/off, `layout`으로 위치를 정한다.

```yaml
  - source: github:quartz-community/graph
    enabled: false            # 끄기
    layout:
      position: right         # left | right | beforeBody | afterBody
      priority: 10            # 숫자가 작을수록 위
```

자주 손대는 컴포넌트:

| 컴포넌트 | 화면에서 보이는 것 |
|---|---|
| `explorer` | 좌측 폴더 트리 |
| `search` | 검색창 |
| `darkmode` / `reader-mode` | 상단 달·책 아이콘 토글 |
| `table-of-contents` | 우측 목차 |
| `graph` / `backlinks` | 우측 그래프·백링크 |
| `tag-list` | 제목 아래 태그 칩 |
| `note-properties` | 속성 표 (`hidePropertiesView: true`로 숨김) |
| `article-title` | 페이지 제목 |
| `breadcrumbs` / `content-meta` | 상단 경로·읽는 시간 |

폴더를 클릭했을 때의 동작도 옵션이다.

```yaml
  - source: github:quartz-community/explorer
    options:
      folderClickBehavior: collapse   # collapse=펼치기, link=폴더 페이지로 이동
      folderDefaultState: open        # open=처음부터 펼침
```

좌측 요소들의 세로 순서는 그룹으로 조절한다. 같은 그룹은 한 줄에 나란히 놓이고, 그룹에서 빼면 단독 행이 된다.

```yaml
layout:
  groups:
    toolbar:
      priority: 15      # 그룹 전체의 세로 위치
      direction: row
```

### 커스텀 CSS 붙이기

기본 테마·옵션으로 부족할 때만 하면 된다. **이 문서에서 가장 손이 많이 가는 부분이니, 급하지 않으면 건너뛰어도 된다.**

여기서 만드는 파일 두 개는 지금까지와 성격이 다르다. 그래서 먼저 짚고 간다.

![[Pasted image 20260728223953.png]]

기준은 **"Publish changes" 버튼이 그 파일을 옮겨주느냐**다. "볼트 것 / GitHub 것"이 아니다.

| 파일 | 발행 버튼이 옮겨주나 | 고치는 방법 |
|---|---|---|
| 노트 (`.md`) | ✅ | 볼트에서 고치고 발행 버튼 |
| `quartz.config.yaml` | ✅ | 볼트 로컬 파일을 고치고 발행 버튼 (저장소 쪽은 매번 덮어써짐) |
| `custom.css` | ❌ | 내가 직접 저장소에 올린다 |
| `.github/workflows/deploy.yml` | ❌ | 위와 같다 |

아래 두 개는 플러그인이 존재 자체를 모른다. 그래서 **덮어쓰지도 않고, 옮겨주지도 않는다.** 한 번 올려두면 계속 살아 있지만 고칠 때마다 내가 올려야 한다.

올리는 방법은 아무거나 된다 — GitHub 웹 편집기, `gh` CLI, 저장소를 clone해서 push, AI 도구에게 시키기.

> [!tip] 로컬에 사본을 두면 편하다
> `custom.css`를 볼트 어딘가에 두고 거기서 고친 뒤 올리면, Obsidian에서 바로 편집할 수 있고 볼트 git 히스토리에도 남는다.
> 단 **발행 폴더 밖**에 둘 것. 발행 폴더 안에 넣으면 노트로 취급돼 사이트에 페이지로 올라간다.

플러그인이 CSS 주입 경로를 제공하지 않으므로 워크플로우를 직접 고쳐야 한다.

먼저 워크플로우 파일 수정 권한을 받는다. 기본 토큰에는 `workflow` 스코프가 없어 GitHub이 거부한다.

```bash
gh auth refresh -h github.com -s workflow
```

저장소 루트에 `custom.css`를 만들고, `.github/workflows/deploy.yml`의 `Build site`와 `upload-pages-artifact` **사이**에 스텝을 넣는다.

```yaml
      - name: Inject custom CSS
        run: |
          cp custom.css quartz-engine/public/custom.css
          find quartz-engine/public -name '*.html' -print0 | xargs -0 sed -i 's#</head>#<link rel="stylesheet" href="/<저장소이름>/custom.css"></head>#'
```

push하면 Actions가 자동으로 재빌드한다. 이 두 파일은 플러그인이 건드리지 않으므로 이후 발행에도 살아남는다.

### CSS 작성 규칙

**색은 반드시 테마 변수로 쓴다.** 하드코딩하면 라이트모드에서만 맞고 다크모드에서 글자가 안 보인다.

| 변수 | 용도 |
|---|---|
| `--light` / `--dark` | 배경 / 본문 글자 |
| `--lightgray` | 옅은 테두리, 표 헤더 배경 |
| `--gray` | 보조선, 비활성 텍스트 |
| `--secondary` | 강조(accent) |
| `--highlight` | 반투명 강조 배경 |

**폰트를 바꾸려면 요소를 명시한다.** Quartz가 `h1`~`h6`, `thead`, `code`, `pre`, 팝오버에 `font-family`를 직접 걸어두기 때문에, `body`에만 지정하면 제목에는 안 먹는다.

```css
body, article, h1, h2, h3, h4, h5, h6, thead, .popover > .popover-inner {
  font-family: 'Pretendard', 'Apple SD Gothic Neo', sans-serif !important;
}
```

자주 쓰는 셀렉터:

- 본문 컨테이너 — `.markdown-preview-view.markdown-rendered`
- 목차 항목 — `.toc-content li.depth-0` ~ `.depth-3`
- 태그 칩 — `a.internal.tag-link`

### 노트 작성 규칙

사이트가 컴포넌트로 자동 렌더하는 것을 본문에 또 쓰면 중복된다.

- **본문에 H1(`# 제목`)을 쓰지 않는다** — `article-title`이 이미 제목을 렌더한다. 쓰면 제목이 두 번 나오고 우측 목차에도 중복 항목이 생긴다
- **본문에 인라인 `#태그`를 쓰지 않는다** — `tag-list`가 frontmatter를 읽어 칩으로 렌더한다
- **제목에 번호를 붙이지 않는다** (`## 1.`, `### 4-1.`) — 좌측 트리와 우측 목차가 제목을 그대로 읽어서 계층이 이중으로 보인다
- 섹션은 `##`부터 시작한다

## 결과물 확인

발행이 끝나면 아래를 눈으로 확인한다.

- 루트 주소가 열리고 홈페이지가 보인다
- 좌측에 폴더 트리가 있고, 폴더를 누르면 하위 노트가 펼쳐진다
- 태그가 칩 한 줄로만 표시된다 (속성 표나 중복 없음)
- 우측 목차가 제목 깊이만큼 들여쓰기된다
- 다크/라이트를 토글해도 표 헤더와 본문이 모두 읽힌다

> [!example] 📸 화면 09 — 완성된 사이트
> **찍을 것:** 발행된 사이트를 브라우저에서 연 전체 화면. 좌측 트리 + 본문 + 우측 목차가 한 화면에 다 보이게.
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/09-완성-사이트.png]]` 로 바꾸면 끝.

> [!example] 📸 화면 10 — 다크·라이트 비교
> **찍을 것:** 같은 페이지를 다크모드와 라이트모드로 각각 캡처. **표가 포함된 화면**이면 가장 좋다 (테마 변수 규칙의 근거가 된다).
> **교체 방법:** 캡처 후 이 콜아웃 전체를 `![[_attachments/10-다크-라이트-비교.png]]` 로 바꾸면 끝.

터미널로 점검하려면:

```bash
gh run list --repo <owner>/<repo> --limit 1      # 빌드 성공 여부
curl -sI https://<owner>.github.io/<repo>/       # 200 확인
```

> [!note]
> URL은 전부 **소문자 슬러그**로 변환된다. `/AI/제목`이 아니라 `/ai/제목`으로 접근해야 한다.

## 기존 방식과 비교

| | Obsidian Publish (공식) | GitHub Publish + Quartz |
|---|---|---|
| 비용 | 월 구독 | 무료 |
| 설정 방식 | GUI 토글 | YAML 편집 |
| 커스터마이징 | 커스텀 CSS 업로드 | CSS + 컴포넌트 on/off + 테마 프리셋 |
| 발행 속도 | 즉시 | 빌드 2~10분 |
| 데이터 보관 | 서비스에 종속 | GitHub 저장소에 그대로 남음 |
| 진입 장벽 | 낮음 | YAML·CSS를 조금 만져야 함 |

## 요약과 Q&A

| 오늘 정리한 내용 | 요약 |
|---|---|
| 개설 | "Set up site"는 최초 1회, 이후는 "Publish changes" |
| 홈페이지 | 발행 폴더 바로 밑 `index.md`가 있어야 생김 |
| 설정 | 볼트 안 로컬 파일을 고칠 것 (저장소 파일은 덮어써짐) |
| 반영 | 설정을 고쳤으면 "Publish changes"를 한 번 더 눌러야 함 |
| 외형 | 테마 프리셋 → 컴포넌트 on/off → 커스텀 CSS 순서로 접근 |
| CSS | 색은 테마 변수로, 폰트는 요소를 명시해서 |
| 노트 | H1·인라인 태그·제목 번호를 쓰지 않기 |

**남은 질문 / 다음 단계**

- [ ] 커스텀 도메인 연결 (`cname` 플러그인)
- [ ] 비공개 노트 제외 규칙 정하기 (`explicit-publish` 또는 `remove-draft`)
- [ ] 노트가 쌓이면 그래프뷰 다시 켜기 (노트가 적으면 빈 화면만 차지한다)

## 자주 빠지는 함정

실제로 부딪혀서 되돌린 것들이다. 증상만 보면 원인을 짐작하기 어려운 것 위주로 모았다.

| 함정 | 증상 | 원인 | 해결 |
|---|---|---|---|
| 리본 지구본 아이콘 클릭 | 발행할 때마다 폴더 선택 마법사 | 그 아이콘은 `setup-site`에 연결돼 있음 | Command palette → "Publish changes" |
| 발행 폴더에 `index.md` 없음 | 루트 주소가 404 | Quartz에 홈페이지로 쓸 소스가 없음 | 폴더 바로 밑에 `index.md` 생성 |
| GitHub에서 `quartz.config.yaml` 수정 | 다음 발행 때 원복됨 | 플러그인이 로컬 파일로 덮어씀 | 볼트의 `sites/<owner>_<repo>/` 파일을 수정 |
| 설정만 고치고 발행 안 함 | 화면이 그대로 | 로컬 파일은 발행해야 올라감 | "Publish changes" 한 번 더 |
| 빈 폴더만 만들고 발행 | 사이트에 폴더가 안 보임 | Git은 빈 디렉터리를 추적하지 않음 | 폴더 안에 파일을 넣고 다시 발행 |
| 테마를 이름만 보고 선택 | 의도와 전혀 다른 색조 | 이름이 겹칠 뿐 무관한 테마일 수 있음 | `--accent-h` 값을 먼저 확인 |
| 본문에 인라인 `#태그` | 태그가 두 번 표시 | `tag-list`가 frontmatter를 이미 렌더 | frontmatter `tags`에만 작성 |
| 본문에 H1 | 제목이 두 번, 목차에도 중복 | `article-title`이 이미 렌더 | 본문은 `##`부터 시작 |
| CSS에 색상 하드코딩 | 다크모드에서 표 헤더가 안 읽힘 | 라이트모드 전용 값이 그대로 적용됨 | `var(--lightgray)` 등 테마 변수 사용 |
| 목차 CSS를 `depth-2`만 지정 | H1을 지우자 계층이 사라짐 | H1 유무로 depth 번호가 한 칸씩 밀림 | `depth-1`~`depth-3` 전 구간을 지정 |
| `!important`로 덮었는데 무변화 | 값을 바꿔도 화면이 그대로 | 기본 규칙과 **같은 변수**로 덮어씀 | 변수가 아니라 실제 색이 뭔지 먼저 확인 |
| 워크플로우 파일 수정이 404 | `gh api PUT`이 실패 | 토큰에 `workflow` 스코프 없음 | `gh auth refresh -h github.com -s workflow` |
| 대문자 경로로 접속 | 빈 페이지 | URL이 소문자로 슬러그화됨 | `/ai/...` 처럼 소문자로 접근 |
