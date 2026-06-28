# gihyun-site — Claude Code 가이드

개인 웹사이트 gihyun.com의 소스 파일 저장소입니다.

## 사이트 구조

```
index.html          ← 메인 피드 페이지
posts/
  [slug]-en.html    ← 영어 버전
  [slug]-ko.html    ← 한국어 버전
images/             ← 대표 이미지
```

## 새 글 작성 규칙

새 글을 만들 때 반드시 세 곳을 수정합니다:
1. `posts/[slug]-en.html` 생성
2. `posts/[slug]-ko.html` 생성
3. `index.html` 피드 상단에 카드 블록 추가 (최신 글이 맨 위)

### 파일명 규칙
- 소문자, 하이픈 구분자 사용 예: `han-river-en.html`

---

## 디자인 기준: maybe-monet 스타일

`posts/maybe-monet-en.html`이 표준 템플릿입니다. 새 글은 이 파일의 CSS를 그대로 사용합니다.

### CSS 색상 변수 (공통)
```css
--bg: #fbfaf7;
--text: #171411;
--muted: #6f6860;
--line: #e3ddd4;
--accent: #7c5b46;
```

---

## 영어 vs 한국어 파일 차이점

| 항목 | 영어 (`-en.html`) | 한국어 (`-ko.html`) |
|---|---|---|
| `<html lang>` | `en` | `ko` |
| body `font-family` | `Georgia, "Times New Roman", serif` | `-apple-system, BlinkMacSystemFont, "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic", Arial, sans-serif` |
| body `line-height` | `1.65` | `1.75` |
| body 추가 속성 | — | `word-break: keep-all; overflow-wrap: break-word;` |
| h1 크기 | `clamp(44px, 7vw, 72px)` | `clamp(40px, 7vw, 68px)` |
| h1 `line-height` | `0.98` | `1.12` |
| h1 `font-weight` | `400` | `500` |
| `.post-body p` 크기 | `20px` | `18px` |
| `.post-body p` `line-height` | `1.72` | `1.9` |
| `.post-body p` `letter-spacing` | — | `-0.015em` |
| 모바일 p 크기 | `18px` | `16.5px` |
| `.language-links` 활성 | `English`에 `class="active"` | `한국어`에 `class="active"` |
| img `alt` | 영어 설명 | 한국어 설명 |
| OG `og:url` | `-en.html` URL | `-ko.html` URL |

---

## 포스트 파일 HTML 구조

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>[제목] — Gihyun Kwon</title>
  <meta name="description" content="[한 줄 설명]" />

  <meta property="og:title" content="[제목]" />
  <meta property="og:description" content="[설명]" />
  <meta property="og:type" content="article" />
  <meta property="og:url" content="https://gihyun.com/posts/[slug]-en.html" />
  <meta property="og:image" content="https://gihyun.com/images/[이미지].jpg" />

  <style>
    /* maybe-monet-en.html의 <style> 블록 그대로 사용 */
    /* 한국어 버전은 위 표 기준으로 font-family 등 교체 */
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <a class="name" href="../index.html">Gihyun Kwon</a>
      <nav aria-label="Main navigation">
        <a href="../index.html#feed">Notes</a>
        <a href="../index.html#about">About</a>
      </nav>
    </header>

    <main>
      <a class="back-link" href="../index.html#feed">← Back to Notes</a>

      <p class="meta">
        <span>카테고리1</span>
        <span class="slash">/</span>
        <span>카테고리2</span>
      </p>

      <h1>[제목]</h1>

      <div class="language-links" aria-label="Language links for [제목] note">
        <a class="active" href="[slug]-en.html">English</a>
        <span class="divider" aria-hidden="true"></span>
        <a href="[slug]-ko.html" lang="ko">한국어</a>
      </div>

      <figure class="post-image">
        <img src="../images/[이미지].jpg" alt="[이미지 설명]" />
      </figure>

      <article class="post-body">
        <p>문단 내용.</p>
      </article>
    </main>

    <footer>
      <div>© Gihyun Kwon</div>
      <div>Seoul · gihyun.com</div>
    </footer>
  </div>
</body>
</html>
```

---

## index.html 카드 블록 구조

`<main class="feed">` 안에 최신 글을 맨 위에 추가합니다.

```html
<article class="note-card">
  <a class="note-image" href="posts/[slug]-en.html" aria-label="Read [제목] note in English">
    <img src="images/[이미지].jpg" alt="[이미지 설명]" />
  </a>

  <div class="note-content">
    <div>
      <p class="meta">
        <span>카테고리1</span>
        <span class="slash">/</span>
        <span>카테고리2</span>
      </p>

      <h2 class="note-title">[제목]</h2>

      <p class="excerpt">[피드에 표시될 한 줄 요약]</p>
    </div>

    <div class="note-footer">
      <time datetime="YYYY-MM">[Month YYYY]</time>

      <div class="language-links" aria-label="Language links for [제목] note">
        <a href="posts/[slug]-en.html">Read in English</a>
        <span class="divider" aria-hidden="true"></span>
        <a href="posts/[slug]-ko.html" lang="ko">한국어로 읽기</a>
      </div>
    </div>
  </div>
</article>
```

---

## 글 작성 요청 방법

사용자가 새 글을 요청할 때 보통 다음 정보를 줍니다:
- 제목 (한국어 또는 영어)
- 카테고리 (1~2개)
- 이미지 파일명
- 글 내용 또는 메모/키워드

내용이 한국어로 주어지면 영문 번역도 함께 작성합니다.
내용이 짧은 메모나 키워드 수준이면 글로 확장해서 작성합니다.
