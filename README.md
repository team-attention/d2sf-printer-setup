# sindoh-printer-setup

신도리코 D451 복합기 Mac 설치 가이드 - 드라이버 다운로드부터 테스트 인쇄까지.

## What it does

Claude Code에서 "프린터 설치", "복합기 설치", "신도리코 설치" 등을 말하면 D451 복합기 설치를 단계별로 안내합니다.

**설치 흐름:**
1. 현재 환경 진단 (macOS 버전, 기존 드라이버, 네트워크)
2. 드라이버 다운로드 안내 (신도리코 홈페이지)
3. 프린터 추가 (`lpadmin`)
4. 기본 프린터 설정
5. 테스트 인쇄

## Install

```bash
claude install-plugin github:team-attention/sindoh-printer-setup
```

## Usage

```
> 프린터 설치해줘
> 신도리코 D451 설치
> 복합기 드라이버 설치
```

## Key Info

| Item | Value |
|------|-------|
| Printer IP | 10.90.36.100 |
| Model | Sindoh D451 / CM3035 |
| Protocol | LPD |
| Driver | SINDOH_D451_CM3035 PS |
| Download | [sindoh.com](https://www.sindoh.com/support/download/d451) |
| D2SF Guide | [Notion](https://www.notion.so/d2sf/D2SF-66394477b8704ccc8febd2ae99fa8c77) |
