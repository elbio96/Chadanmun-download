# 차단문 다운로드 페이지

스마트스토어 구매자가 설치파일을 받아가는 공식 다운로드 페이지입니다.
`docs/index.html` 한 개로 동작하며 빌드 과정이나 외부 라이브러리가 없습니다.

이 저장소는 **배포용 공개 저장소**입니다. 페이지와 설치파일만 두고,
프로그램 소스는 비공개 저장소 `elbio96/Chadanmun` 에 있습니다.

```
├── docs/
│   └── index.html   ← 페이지 전체 (HTML + CSS + JS)
└── README.md        ← 이 문서
```

---

## GitHub Pages 에 올리는 방법

1. 이 저장소를 GitHub 에 올립니다.
2. 저장소 페이지에서 **Settings → Pages** 로 이동합니다.
3. **Source** 를 `Deploy from a branch` 로 둡니다.
4. **Branch** 를 `main`, 폴더를 `/docs` 로 선택하고 **Save** 를 누릅니다. (설정 완료됨)
5. 1~2분 뒤 아래 주소로 페이지가 열립니다.

```
https://elbio96.github.io/Chadanmun-download/
```

이 주소를 스마트스토어 상품 안내나 발송 메시지에 넣으면 됩니다.

---

## 설치파일 올리는 방법 (GitHub Releases)

페이지의 다운로드 버튼은 GitHub Releases 에 올린 파일을 가리킵니다.

1. 저장소 페이지에서 **Releases → Draft a new release** 를 누릅니다.
2. **Tag** 에 `v1.1.1` 처럼 `v` + 버전 번호를 입력합니다.
3. `Chadanmun_Setup_1.1.1.exe` 파일을 첨부합니다.
4. **Publish release** 를 누릅니다.

태그 이름과 파일명 규칙만 지키면 페이지가 자동으로 올바른 주소를 만듭니다.

| 항목 | 규칙 | 예시 |
|---|---|---|
| 태그 | `v` + 버전 | `v1.1.1` |
| 파일명 | `Chadanmun_Setup_` + 버전 + `.exe` | `Chadanmun_Setup_1.1.1.exe` |

완성되는 다운로드 주소:

```
https://github.com/elbio96/Chadanmun-download/releases/download/v1.1.1/Chadanmun_Setup_1.1.1.exe
```

---

## 수정해야 하는 위치

`index.html` 맨 아래 `<script>` 안, 주석으로 표시된 두 줄이 전부입니다.

```js
const REPO = 'elbio96/Chadanmun-download';
const VERSION = '1.0.1';
```

| 바꾸는 값 | 언제 | 반영되는 곳 |
|---|---|---|
| `REPO` | 저장소를 옮길 때만 (지금은 설정 완료) | 다운로드 주소 |
| `VERSION` | 새 버전 배포할 때마다 | 다운로드 주소, 현재 버전 표시, 설치파일명, 하단 버전 정보 |

설치파일명과 다운로드 주소는 이 두 값에서 자동으로 만들어지므로 따로 고칠 필요가 없습니다.

---

## 새 버전을 배포할 때

예를 들어 `1.0.2` 를 내보낸다면:

1. `packaging\build.ps1` 로 설치파일을 빌드합니다.
2. GitHub 에서 `v1.0.2` 태그로 릴리스를 만들고 `Chadanmun_Setup_1.0.2.exe` 를 첨부합니다.
3. `index.html` 에서 `VERSION` 을 `1.0.2` 로 바꿉니다.
4. 커밋하고 push 합니다. 1~2분 뒤 페이지에 반영됩니다.
5. 페이지를 열어 다운로드 버튼이 실제로 파일을 받아오는지 한 번 확인합니다.

버전 번호는 `index.html`, `packaging\build.ps1`, `packaging\Chadanmun.iss`,
`packaging\Launcher.cs`, `packaging\Chadanmun.exe.manifest` 에 각각 들어 있습니다.
설치파일 쪽 네 개는 빌드 전에, `index.html` 은 릴리스를 올린 뒤에 바꿉니다.

---

## 참고

- 설치파일 자체는 이 폴더에 넣지 않습니다. 용량이 크고, GitHub Releases 가 배포를 맡습니다.
- 페이지는 JavaScript 로 버전과 파일명을 채웁니다. JavaScript 가 꺼져 있으면 안내 문구가 대신 표시됩니다.
- 파일 크기와 SHA-256 값은 소스 저장소의 `packaging\dist\RELEASE.txt` 에 있습니다.
