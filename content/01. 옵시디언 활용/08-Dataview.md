---
title: "08. Dataview — 노트 더미를 표·대시보드로"
tags: [obsidian, 강의, 사내교육, 핸즈온]
created: 2026-07-30
updated: 2026-07-30
---

## 한 줄로 말하면

노트의 속성(frontmatter)을 데이터베이스처럼 조회한다. 코드 네 줄로 자동 갱신되는 표가 생긴다.

## 왜 필요한가

노트가 100개를 넘으면 사람이 못 훑는다. **읽는 노트에서 조회하는 노트로** 넘어가야 하는 지점이다.

독서 대시보드, 진행 중인 프로젝트 목록, 흩어진 미완료 할 일이 손대지 않아도 한 곳에 모인다. 새 노트를 만들면 표에 행이 저절로 늘어난다.

## 설치와 세팅

1. 커뮤니티 플러그인 → **Dataview** 설치
2. 설정 → `Enable JavaScript Queries` ON, `Enable Inline Queries` ON

> [!note] 순서가 중요하다
> Dataview는 속성이 있어야 동작한다. 그 속성을 6회차 Linter와 7회차 Templater가 만들어 준다. **순서를 바꾸면 이 회차 시연이 빈 표로 나온다.**

## 라이브 시연

`대시보드.md`를 만들고 아래를 넣는다.

````markdown
```dataview
TABLE created AS "작성일", type AS "종류"
FROM "03.일지"
SORT created DESC
LIMIT 10
```
````

흩어진 미완료 할 일을 전부 모으는 쿼리도 보여준다.

````markdown
```dataview
TASK
WHERE !completed
GROUP BY file.link
```
````

1. 표가 자동 생성된다 → 새 노트를 하나 더 만들고 대시보드로 돌아오면 **행이 늘어 있다**
2. TASK 쿼리 결과에서 체크박스를 클릭한다 → **원본 노트의 체크박스가 같이 바뀐다.** 이 회차의 하이라이트다

## 직접 해보기 (5분)

1. `대시보드.md`를 만든다
2. 위 TABLE 쿼리를 복사해 붙인다
3. `FROM` 뒤 폴더명만 자기 폴더로 고친다

## 흔한 함정

> [!warning] 속성 없는 노트는 쿼리에 안 잡힌다
> "왜 표가 비어 있죠?"의 1순위 원인이다. Linter의 `Insert YAML attributes`를 켜두면 저장할 때 알아서 채워진다.

> [!warning] 폴더명은 큰따옴표 필수
> `FROM "03.일지"` — 따옴표를 빼면 조용히 아무것도 안 나온다.

> [!warning] 이 표는 옵시디언 안에서만 보인다
> GitHub나 웹에 그대로 올리면 코드블록으로 보인다. 공유용 문서에는 결과를 복사해 넣어야 한다.

---
← [[07-Templater]] · 다음 → [[09-Web-Clipper]] · [[00-강의-목차|강의 목차]]
