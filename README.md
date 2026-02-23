# d2sf-printer-setup

Naver D2SF 입주 공간 프린터 세팅이 너무 귀찮아서 만든 Claude Code 플러그인.

새로 입주하면 프린터 드라이버 찾고, IP 넣고, PPD 선택하고... 이걸 매번 수동으로? "프린터 설치해줘" 한마디면 끝.

## What it does

Claude Code에서 "프린터 설치해줘"라고 말하면:

1. macOS 환경 자동 진단
2. 신도리코 D451 드라이버 설치 안내
3. 프린터 추가 (`lpadmin`)
4. 기본 프린터 설정
5. 테스트 인쇄

## Install

```bash
claude install-plugin github:team-attention/d2sf-printer-setup
```

## Usage

```
> 프린터 설치해줘
> 프린터가 안 돼요
> 복합기 설치
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
