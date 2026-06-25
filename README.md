# Calcifier

실내 캠핑용 스마트 걸이 램프 **Calcifier**의 MCP 서버 및 가상 디바이스 쇼룸 프로젝트입니다.
Claude와의 대화로 램프를 제어하고, 브라우저 쇼룸에 실시간으로 반영됩니다.

---

## 파일 구조

```
calcifier/
├── index.html              ← 브랜드 웹페이지 (GitHub Pages 진입점)
├── calcifier.html          ← 가상 디바이스 쇼룸 (MCP 연동 목업)
├── sound.mp3               ← 장작 모닥불 사운드
├── src/
│   └── index.ts            ← MCP 서버 본체 (Cloudflare Workers)
├── wrangler.toml           ← Cloudflare Workers 배포 설정
├── package.json
├── tsconfig.json
└── .github/
    └── workflows/
        └── deploy.yml      ← GitHub Actions 자동 배포
```

---

## 학생 작업 순서

### 1단계 — claude.ai 커스텀 커넥터 연결

1. claude.ai → **Settings → Integrations → Add custom integration**
2. URL 입력: `https://calcifier.typica-918.workers.dev/mcp`
3. OAuth 인증 팝업 → 허용
4. 도구 목록에서 아래 9개 tool이 보이면 연결 성공

| Tool | 기능 |
|---|---|
| `get_product_info` | 제품 소개 |
| `get_lamp_state` | 현재 상태 조회 |
| `set_power` | 전원 ON/OFF |
| `set_brightness` | 밝기 설정 |
| `set_color_temp` | 색온도 설정 |
| `set_mode` | 모드 전환 (일반/취침/독서/기상) |
| `set_volume` | 장작 사운드 볼륨 |
| `set_tent_size` | 텐트 크기 변경 + 밝기 자동 조정 |
| `charge_battery` | 배터리 100% 완충 |

---

### 2단계 — index.html 브랜드 페이지 작성

`index.html`을 팀의 브랜드 웹페이지로 자유롭게 작성하세요.
폰트, 컬러, 레이아웃 등 디자인 방향은 팀이 직접 결정합니다.
GitHub Pages를 통해 `https://{계정명}.github.io/{레포명}/`으로 퍼블리싱됩니다.

---

### 3단계 — calcifier.html 룩앤필 수정

`calcifier.html`의 디자인을 **자신이 작성한 `index.html`의 스타일과 일치**하도록 수정하세요.
폰트, 컬러, 타이포그래피 등을 맞추어 두 페이지가 같은 브랜드처럼 보이게 합니다.
전체 레이아웃이나 기능 로직은 수정하지 않아도 됩니다.

---

### 4단계 — 쇼룸에서 MCP 연동 확인

`calcifier.html`을 브라우저에서 열고 하단 **MCP Worker URL** 입력창에 아래 주소를 입력하세요.

```
https://calcifier.typica-918.workers.dev
```

> ⚠️ **주의: URL 끝에 `/`를 붙이지 마세요.**
> 끝에 `/`가 있으면 `/state` 폴링 경로가 `//state`로 잘못 조합되어 연결에 실패합니다.
> 올바른 예: `https://calcifier.typica-918.workers.dev`
> 잘못된 예: `https://calcifier.typica-918.workers.dev/`

**실시간 폴링 시작** 버튼을 누른 뒤, Claude에게 자연어로 명령해보세요.

```
"잘게."            → 취침 모드
"책 읽을게."       → 독서 모드
"밝기 50%로 해줘." → 밝기 조절
"충전해줘."        → 배터리 100%
```

---

## 자주 발생하는 오류

| 오류 | 원인 | 해결 |
|---|---|---|
| 폴링은 되는데 상태가 안 바뀜 | Claude가 tool을 아직 호출 안 함 | Claude에게 직접 명령 후 확인 |
| 쇼룸이 서버와 연결 안 됨 | Worker URL 끝에 `/` 포함 | URL 끝 `/` 제거 |
