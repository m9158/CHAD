# 배포 가이드

> 현재 상태: `board.html` 은 **서버에 올라가 있지 않다.**
> `index.html` 도 탭이 2개인 예전 버전이다.

---

## 0. 먼저 확인할 것 — 지금 어디에 배포되고 있나

아무도 모르는 상태면 여기서 시작한다. 브라우저에서 30초면 된다.

1. <https://caucalendarcn.co.kr> 접속
2. `F12` → **Network** 탭 → 새로고침
3. 목록 맨 위 요청 클릭 → **Response Headers** 확인

| 헤더 | 호스팅 | 배포 방법 |
|---|---|---|
| `x-vercel-id` | Vercel | GitHub push → 자동 |
| `x-nf-request-id` | Netlify | GitHub push → 자동 |
| `server: GitHub.com` | GitHub Pages | GitHub push → 자동 |
| `cf-ray` + `server: cloudflare` | Cloudflare Pages | GitHub push → 자동 |
| `server: nginx` / `Apache` 만 | 국내 호스팅 | FTP 또는 파일매니저로 직접 업로드 |

### 팀원에게 물어볼 것 (그대로 복사해서 보내기)

```
사이트 배포 관련해서 확인 좀 부탁해!

1. caucalendarcn.co.kr 은 어디에 올라가 있어? (Vercel / Netlify / GitHub Pages / 카페24 같은 호스팅)
2. 파일은 어떻게 올려? (git push / FTP / 관리자 페이지 업로드)
3. 그 계정 접근 권한 나도 받을 수 있을까? 아니면 로그인 정보 공유 가능한지
4. 도메인(caucalendarcn.co.kr)은 어디서 샀고 누구 계정으로 관리돼?
5. 소스코드 저장소(GitHub 등)가 이미 있어? 있으면 링크

게시판 페이지 추가하려고 하는데 배포 경로를 몰라서 막혀 있어.
```

---

## 1. 이번에 올려야 하는 파일

| 파일 | 상태 | 비고 |
|---|---|---|
| `board.html` | 신규 | 게시판 |
| `styles.css` | 변경 없음 | 이미 서버에 있으면 **안 올려도 됨** |
| `index.html` | 1줄 수정 | 탭에 게시판 링크 추가 |
| `reviews.html` | 1줄 수정 | 탭에 게시판 링크 추가 |

### 탭 링크 추가 (index / reviews 두 파일 모두)

`<div class="tabs">` 안에서, 마지막 `</a>` 다음에 이 한 줄을 추가한다.

```html
<a href="board.html" class="tab-link">广场 · 게시판</a>
```

수정 후 이런 모양이 된다.

```html
<div class="tabs">
  <a href="index.html" class="tab-link active">학사일정 · 日历</a>
  <a href="reviews.html" class="tab-link">강의평 · 点评</a>
  <a href="board.html" class="tab-link">广场 · 게시판</a>
</div>
```

---

## 2. DB 먼저 — 안 하면 배포해도 에러만 뜬다

파일을 올리기 **전에** Supabase 설정을 끝내야 한다.
안 하면 게시판에 `加载失败: Could not find the table 'public.posts_public'` 이 뜬다.

1. <https://supabase.com/dashboard> 로그인 → 프로젝트 `ltmvvsmraejnuboyjarx` 선택
2. 왼쪽 **SQL Editor** → **New query**
3. `db/schema_board.sql` 전체 붙여넣고 **Run**
   - 성공하면 `Success. No rows returned` 가 뜬다
   - 여러 번 실행해도 안전하다
4. **Table Editor** 에서 `posts`, `comments`, `profiles`, `post_likes`, `reports` 5개가 보이는지 확인
5. **Authentication → Providers → Email** 활성화
   - `Confirm email` 은 **끄는 것을 권장** (켜면 메일 인증 전까지 로그인 불가 → 초기 이탈)
6. **Authentication → URL Configuration → Site URL** 에 `https://caucalendarcn.co.kr` 입력

---

## 3. 배포하기

### A. GitHub 연동인 경우 (Vercel / Netlify / Pages / Cloudflare)

```bash
git pull                       # 먼저 최신 상태로
cp ~/받은파일/board.html .
# index / reviews 의 탭 링크 수정

git add .
git commit -m "feat: 자유게시판 추가"
git push
```

push 하면 1~2분 내 자동 반영된다. 대시보드의 Deployments 에서 성공 여부를 확인한다.

### B. 국내 호스팅 (FTP / 파일매니저)

1. 호스팅 관리자 페이지 → 파일매니저 (또는 FileZilla 같은 FTP 클라이언트)
2. 웹 루트 폴더로 이동 — 보통 `/public_html`, `/www`, `/htdocs` 중 하나.
   `index.html` 이 있는 폴더가 맞다
3. `board.html` 업로드
4. `index.html`, `reviews.html` 을 수정본으로 덮어쓰기

**덮어쓰기 전에 기존 파일을 반드시 백업한다.** 되돌릴 방법이 없다.

---

## 4. 배포 후 확인 (체크리스트)

- [ ] <https://caucalendarcn.co.kr/board.html> 접속 시 목록 화면이 뜬다
- [ ] 학사일정·강의평 페이지 상단에 `广场` 탭이 보이고 클릭하면 이동한다
- [ ] 게시판에서 회원가입 → 로그인이 된다
- [ ] 글 작성 → 목록에 나타난다
- [ ] 댓글 작성 → `匿名1` 번호가 붙는다
- [ ] 공감 버튼이 토글된다
- [ ] **휴대폰에서** 위 전부 확인 (유학생 트래픽은 대부분 모바일)
- [ ] 로그아웃 상태에서 글쓰기 누르면 로그인 창이 뜬다

### 화면이 안 바뀌면

브라우저 캐시일 가능성이 높다. `Ctrl + Shift + R` (Mac은 `Cmd + Shift + R`) 로 강력 새로고침.
그래도 그대로면 파일이 실제로 안 올라간 것이니 서버의 파일 목록을 직접 확인한다.

---

## 5. 다음부터는 이렇게 하자 — GitHub 기반으로 통일

지금은 "누가 어떻게 올렸는지 아무도 모르는" 상태다. 팀으로 개발하는 이상 이건 계속 문제가 된다.
누가 무엇을 언제 바꿨는지 알 수 없고, 되돌릴 수도 없고, 배포 권한이 한 사람에게 묶인다.

**권장: GitHub 저장소 + Vercel 연결.** 무료이고 15분이면 끝난다.

1. GitHub에 저장소 생성 → 현재 파일 전부 push
2. <https://vercel.com> 가입 → **Add New Project** → 저장소 선택 → Deploy
   (정적 HTML이라 설정 건드릴 것 없음)
3. Vercel 프로젝트 → **Settings → Domains** → `caucalendarcn.co.kr` 추가
4. 안내에 따라 도메인 등록업체(가비아·후이즈 등)에서 DNS 레코드 변경
5. 이후 배포는 `git push` 뿐이다

이렇게 하면 팀원 누구나 PR을 올릴 수 있고, 배포 전 미리보기 링크가 자동 생성되고,
문제가 생기면 이전 버전으로 한 번에 되돌릴 수 있다.
