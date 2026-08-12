# GitHub 사용 가이드 (CHAD 프로젝트)

GitHub를 처음 쓰는 팀원 기준으로 썼다. 순서대로 따라 하면 된다.

---

## 1. 개념부터 — 5분

용어가 낯설어서 어려운 것뿐이다. 실제로는 단순하다.

| 용어 | 뜻 | 비유 |
|---|---|---|
| **Repository (저장소)** | 프로젝트 파일 전체가 사는 곳 | 프로젝트 폴더 |
| **Clone (클론)** | 저장소를 내 컴퓨터로 복사 | 폴더 통째로 내려받기 |
| **Branch (브랜치)** | 원본을 건드리지 않는 작업용 복사본 | 문서 "사본 만들어 수정하기" |
| **Commit (커밋)** | 변경 사항을 기록으로 남김 | 저장 + 메모 남기기 |
| **Push (푸시)** | 내 컴퓨터의 커밋을 GitHub로 올림 | 업로드 |
| **Pull (풀)** | GitHub의 최신 내용을 내 컴퓨터로 받음 | 다운로드 |
| **Pull Request (PR)** | "이 브랜치를 합쳐도 될까요?" 하는 요청 | 결재 올리기 |
| **Merge (머지)** | PR을 승인해서 실제로 합침 | 결재 승인 |

**왜 브랜치를 쓰나?** 두 사람이 동시에 `main`을 고치면 서로의 작업을 덮어쓴다.
각자 브랜치에서 작업하고 PR로 합치면 그럴 일이 없고, 합치기 전에 서로 확인할 수 있다.

**핵심 흐름은 이 한 줄이다.**

```
pull → 브랜치 생성 → 작업 → commit → push → PR → 리뷰 → merge
```

---

## 2. 준비 — GitHub Desktop 설치 (권장)

명령어가 익숙하지 않다면 **GitHub Desktop**을 쓰는 게 훨씬 쉽다. 버튼만 누르면 된다.

1. <https://desktop.github.com> 접속 → 다운로드 → 설치
2. 실행 → **Sign in to GitHub.com** → 로그인
3. 이름/이메일 확인 화면이 나오면 그대로 진행

> 터미널이 편하면 `git` 명령어를 써도 된다. 아래에 두 방법을 같이 적어뒀다.

---

## 3. 저장소 내려받기 (최초 1회)

### GitHub Desktop

1. `File` → `Clone repository`
2. **GitHub.com** 탭에서 `m9158/CHAD` 선택
   - 안 보이면 URL 탭에 `https://github.com/m9158/CHAD` 입력
3. **Local path** — 저장할 위치 지정 (예: `C:\Users\user\Documents\CHAD`)
4. `Clone` 클릭

### 명령어

```bash
cd ~/Documents
git clone https://github.com/m9158/CHAD.git
cd CHAD
```

이제 그 폴더가 저장소다. 여기서 파일을 수정한다.

---

## 4. 작업 사이클 — 매번 반복하는 것

### 4-1. 시작 전: 최신 상태로 맞추기

**항상 이걸 먼저 한다.** 안 하면 나중에 충돌이 난다.

- **Desktop**: 상단 `Fetch origin` 클릭 → `Pull origin` 이 뜨면 클릭
- **명령어**: `git checkout main` 후 `git pull`

### 4-2. 브랜치 만들기

- **Desktop**: 상단 `Current branch` 클릭 → `New branch` → 이름 입력 → `Create branch`
- **명령어**: `git checkout -b feat/board`

**브랜치 이름 규칙**

```
feat/board-notification     새 기능
fix/anon-number-duplicate   버그 수정
docs/readme-update          문서
```

### 4-3. 파일 수정

평소처럼 편집기에서 수정하고 저장하면 된다. 특별할 게 없다.

### 4-4. 커밋 (기록 남기기)

- **Desktop**: 왼쪽에 변경된 파일 목록이 자동으로 뜬다 → 아래 **Summary** 칸에 메시지 입력 →
  `Commit to feat/board` 클릭
- **명령어**:
  ```bash
  git add .
  git commit -m "feat: 자유게시판 추가"
  ```

**커밋 메시지 형식**

```
feat: 자유게시판 추가              새 기능
fix: 익명 번호 중복 부여 수정       버그
docs: README에 배포 절차 추가       문서
style: 게시판 여백 정리             디자인만
```

한 커밋에 한 가지 일만. "이것저것 수정" 같은 메시지는 나중에 아무 도움이 안 된다.

### 4-5. 푸시 (GitHub로 올리기)

- **Desktop**: `Publish branch` (또는 `Push origin`) 클릭
- **명령어**: `git push -u origin feat/board`

---

## 5. Pull Request 만들기

푸시하면 GitHub 저장소 페이지에 노란 띠로 **"Compare & pull request"** 버튼이 뜬다.

1. 그 버튼 클릭 (또는 `Pull requests` 탭 → `New pull request`)
2. **제목**: 뭘 했는지 한 줄 (예: `자유게시판 추가`)
3. **설명**: 아래 형식이면 충분하다

```
## 무엇을
자유게시판 페이지 추가 (board.html)

## 왜
커뮤니티 기능 1단계

## 확인 방법
1. 로컬에서 board.html 열기
2. 로그인 → 글 작성 → 댓글 확인

## 참고
Supabase에 schema_board.sql 먼저 실행 필요

Closes #3
```

4. 오른쪽 **Reviewers** 에서 상대를 지정
5. `Create pull request` 클릭

> `Closes #3` 이라고 쓰면 머지할 때 3번 이슈가 자동으로 닫힌다.

---

## 6. 리뷰하고 머지하기

**리뷰하는 쪽 (받은 사람)**

1. `Pull requests` 탭 → 해당 PR 클릭
2. **Files changed** 탭 — 초록색이 추가된 줄, 빨간색이 삭제된 줄
3. 특정 줄에 의견이 있으면 줄 번호 옆 `+` 클릭 → 코멘트 작성
4. 문제없으면 **Review changes** → `Approve` → `Submit review`

**머지하는 쪽 (저장소 주인)**

1. PR 페이지 하단 초록색 **Merge pull request** 클릭
2. **Confirm merge** 클릭
3. **Delete branch** 클릭 (선택. 정리 차원에서 하는 게 좋다)

머지 후에는 각자 `main`으로 돌아가서 최신 상태를 받는다.

- **Desktop**: `Current branch` → `main` 선택 → `Pull origin`
- **명령어**: `git checkout main && git pull`

---

## 7. Issues — 할 일 관리

README 체크박스 대신 Issues를 쓰면 "누가 뭘 하고 있는지" 묻는 대화가 사라진다.

**만들기**

1. 저장소 → `Issues` 탭 → `New issue`
2. 제목: 할 일 하나 (예: `도배 방지 트리거 추가`)
3. 본문: 왜 필요한지, 어떻게 확인할지
4. 오른쪽 **Assignees** — 담당자 지정
5. **Labels** — `bug` / `enhancement` / `documentation` 등

**닫기** — PR 설명에 `Closes #3` 을 쓰면 머지할 때 자동으로 닫힌다. 수동으로 `Close issue` 를 눌러도 된다.

**지금 만들어두면 좋을 이슈**

```
#1 Supabase 게시판 스키마 실행
#2 게시판 페이지 배포
#3 도배 방지 트리거 추가
#4 강의평에 로그인 적용
#5 이용약관·개인정보처리방침 작성
#6 모바일 실기기 테스트
#7 댓글 알림 기능
```

---

## 8. Vercel 미리보기 — 가장 유용한 기능

Vercel이 이 저장소에 연결돼 있으면, **PR을 올릴 때마다 미리보기 사이트가 자동 생성된다.**

PR 페이지에 Vercel 봇이 코멘트를 남기고, 거기에 `chad-git-feat-board-xxx.vercel.app` 같은
주소가 붙는다. 그걸 클릭하면 **합치기 전에 실제로 동작하는 화면을 볼 수 있다.**

- 실서비스에 영향을 주지 않는다
- 팀원에게 링크만 보내서 확인받을 수 있다
- 모바일에서도 열어볼 수 있다

"올려봐야 아는" 상황이 없어진다. 저장소를 하나로 통일해야 작동한다.

---

## 9. 자주 겪는 문제

### push 했는데 거부당함 (`rejected`)

내가 작업하는 동안 다른 사람이 먼저 올렸다는 뜻이다.

```bash
git pull --rebase
git push
```

Desktop이면 `Pull origin` 누르고 다시 `Push origin`.

### 충돌 (conflict)이 났다

두 사람이 **같은 파일의 같은 줄**을 고쳤을 때 난다. 파일을 열면 이렇게 표시된다.

```
<<<<<<< HEAD
내가 쓴 내용
=======
상대가 쓴 내용
>>>>>>> main
```

셋 중 하나를 고른다 — 내 것만 남기기 / 상대 것만 남기기 / 둘을 합치기.
`<<<<<<<`, `=======`, `>>>>>>>` 표시줄은 **전부 지운다.** 그 뒤 다시 커밋하면 된다.

무섭게 생겼지만 파일이 깨진 게 아니다. 그냥 둘 중 뭘 쓸지 정해달라는 것뿐이다.

### 초대를 수락했는데 권한이 없다

GitHub 초대는 **7일 후 만료**된다. 저장소 → `Settings` → `Collaborators` 에서
`Pending invitations` 를 확인하고, 남아 있으면 취소 후 다시 초대한다.

**이메일보다 GitHub 사용자명으로 초대하는 게 확실하다.** 이메일은 오타가 나도
초대가 발송돼버려서, 엉뚱한 주소로 가고도 모르게 된다.

### 실수로 main에 직접 커밋했다

아직 push 전이라면 되돌릴 수 있다.

```bash
git reset --soft HEAD~1     # 커밋만 취소, 파일 수정 내용은 남음
```

이미 push 했다면 상대에게 알리고 같이 정리한다. 혼자 `--force` 로 밀지 않는다.

---

## 10. 절대 하지 말 것

**`service_role` 키, DB 비밀번호를 커밋하지 않는다.**

이 저장소는 **Public**이다. 한 번 커밋되면 파일을 지워도 커밋 기록에 영구히 남고,
누구나 볼 수 있다. 그렇게 되면 키를 새로 발급받는 것 외엔 방법이 없다.

`anon` / `publishable` 키는 괜찮다. 클라이언트에 노출되도록 설계된 키이고,
실제 접근 권한은 Supabase의 RLS 정책이 막는다.

**`main` 에 직접 push 하지 않는다.** 항상 브랜치 + PR.
지금은 규칙으로 강제하지 않지만, 서로 지키기로 한다.
