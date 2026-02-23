# d2sf-printer-setup

Naver D2SF 입주 공간의 신도리코 복합기(D451/CM3035) 설치를 자동화하는 AI Agent Skill입니다.

새로 입주할 때마다 드라이버 찾고, IP 넣고, PPD 선택하는 반복 작업을 줄이기 위해 만들었습니다.  
"프린터 설치해줘" 한마디로 macOS 프린터 세팅 플로우를 안내/실행합니다.

## Supported Platforms

이 저장소는 특정 에이전트 전용이 아니라, **Skills를 읽는 모든 에이전트 플랫폼**에서 사용할 수 있습니다.

- Codex
- Claude Code
- 기타 Skills 호환 에이전트 (예: Cursor, Copilot 계열 워크플로우 등)

## What it does

에이전트가 다음 작업을 순서대로 수행합니다.

1. macOS 환경 진단
2. 신도리코 D451 드라이버 설치 확인/안내
3. 프린터 추가 (`lpadmin`)
4. 기본 프린터 설정
5. 테스트 인쇄 및 상태 확인

## Install (skills.sh)

`skills.sh` 문서 기준으로 `npx skills add` 명령으로 설치할 수 있습니다.

```bash
npx skills add https://github.com/team-attention/d2sf-printer-setup --skill setup
```

필요하면 자동 확인 없이 설치:

```bash
npx skills add https://github.com/team-attention/d2sf-printer-setup --skill setup -y
```

참고 문서: https://skills.sh/docs

## AI Agent Install Guide (Agent-Readable)

아래 절차는 AI Agent가 README를 읽고 그대로 실행할 수 있도록 작성된 가이드입니다.

1. Skill 설치
```bash
npx skills add https://github.com/team-attention/d2sf-printer-setup --skill setup -y
```

2. 설치 확인 (선택)
```bash
npx skills list
```

3. 사용자 요청이 아래 의도 중 하나이면 `setup` 스킬을 우선 사용
- `프린터 설치`
- `복합기 설치`
- `신도리코 설치`
- `프린터 연결`
- `printer setup`
- `sindoh setup`

4. 스킬 실행 중 기본 프린터 IP는 `10.90.36.100`을 사용하고, 연결 실패 시 사용자에게 IP를 확인

5. 드라이버 다운로드는 브라우저 기반으로 안내
- 다운로드 페이지: `https://www.sindoh.com/support/download/d451`

## Usage

```text
프린터 설치해줘
프린터가 안 돼요
복합기 설치
```

## D2SF Printer Info

| Item | Value |
|------|-------|
| Printer IP | 10.90.36.100 |
| Model | Sindoh D451 / CM3035 |
| Protocol | LPD |
| Driver | SINDOH_D451_CM3035 PS |
| Download | [sindoh.com](https://www.sindoh.com/support/download/d451) |
| D2SF Guide | [Notion](https://www.notion.so/d2sf/D2SF-66394477b8704ccc8febd2ae99fa8c77) |
