---
name: d2sf-printer-setup
description: This skill should be used when the user asks to "프린터 설치", "복합기 설치", "신도리코 설치", "프린터 연결", "프린터 세팅", "프린터가 안 돼요", "인쇄가 안 돼요", "출력이 안 돼요", "복사기 설치", "프린트 설치", "printer setup", "sindoh setup", "D451 설치", "프린터 추가", "복합기 드라이버", or needs help installing a Sindoh D451 multifunction printer on macOS.
---

# Sindoh D451 Printer Setup

Install the Sindoh D451 (CM3035) multifunction printer on macOS. Follow these steps in order.

## Configuration

- **Default Printer IP**: 10.90.36.100 (D2SF 사무실 기본값)
- If the default IP does not respond to ping, ask the user: "프린터 IP 주소를 알고 계신가요? 기본값은 10.90.36.100입니다."

## Step 1: Check Current State

Run these diagnostic commands to understand the current environment:

```bash
# macOS 버전 확인
sw_vers

# 기존 설치된 프린터 확인
lpstat -a 2>/dev/null || echo "프린터 없음"

# 신도리코 드라이버 설치 여부 확인
lpinfo -m 2>/dev/null | grep -i 'sindoh.*451\|sindoh.*CM3035'

# 프린터 네트워크 연결 확인 (IP는 Configuration 참고)
ping -c 2 -W 2 10.90.36.100 2>/dev/null
```

If ping fails, check if the user is on the correct network (office Wi-Fi/LAN). Ask: "사무실 네트워크(Wi-Fi 또는 유선)에 연결되어 있으신가요?"

## Step 2: Driver Installation

Check if the D451 PPD is already available:

```bash
lpinfo -m 2>/dev/null | grep -i 'sindoh.*451\|sindoh.*CM3035'
```

**If D451 PPD is found**, skip to Step 3.

**If D451 PPD is NOT found**, guide the user to download the driver:

1. Open the download page automatically:
   ```bash
   open "https://www.sindoh.com/support/download/d451"
   ```
2. Based on `sw_vers` output, tell the user which OS to select:
   - macOS 15.x → "Mac OS 15.X"
   - macOS 14.x → "Mac OS 14.X"
   - macOS 13.x → "Mac OS 13.X"
   - If version is not listed, advise: "해당 macOS 버전의 드라이버가 없습니다. IT 담당자에게 문의해주세요."
3. Download and install the **SINDOH_D450_CM.PKG** file (D451과 D450은 같은 드라이버를 사용합니다)

**IMPORTANT**: The download page loads dynamically via JavaScript. Direct API access is blocked by Cloudflare. The user MUST use a browser to download.

**STOP and WAIT here.** Ask the user: "드라이버 설치가 완료되면 '설치 완료'라고 말씀해주세요."

After the user confirms, verify the PPD is available:
```bash
lpinfo -m 2>/dev/null | grep -i 'sindoh.*451\|sindoh.*CM3035'
```

Expected output: `Library/Printers/PPDs/Contents/Resources/SINDOH_D451_CM3035.gz SINDOH D451/CM3035 PS`

If PPD is still not found, the installation may have failed. Ask the user to try again.

## Step 3: Add Printer

Add the printer using `lpadmin`. If the command fails with a permission error, retry with `sudo` and tell the user they may need to enter their Mac login password.

```bash
lpadmin -p "SINDOH_D451" \
  -E \
  -v "lpd://10.90.36.100" \
  -m "Library/Printers/PPDs/Contents/Resources/SINDOH_D451_CM3035.gz" \
  -D "신도리코 D451" \
  -L "사무실"
```

Verify the printer was added:

```bash
lpstat -p SINDOH_D451 -l
```

## Step 4: Set as Default Printer

Before changing the default printer, check the current setting and ask:

```bash
lpstat -d
```

Ask the user: "현재 기본 프린터가 [현재값]으로 설정되어 있습니다. 신도리코 D451을 기본 프린터로 변경하시겠습니까?"

If the user agrees:
```bash
lpoptions -d SINDOH_D451
```

## Step 5: Test Print

Print a test page to verify the setup:

```bash
echo "Sindoh D451 Test Print - $(date)" | lp -d SINDOH_D451
```

Or if the user has a specific file to print:
```bash
lp -d SINDOH_D451 /path/to/user-file.pdf
```

Check print job status:

```bash
lpstat -W all | grep SINDOH_D451
```

After test print, tell the user: "테스트 인쇄가 전송되었습니다. 복합기에서 출력물이 나오는지 확인해주세요."

## Troubleshooting

If the printer doesn't respond:

```bash
# 네트워크 연결 확인
ping -c 3 10.90.36.100

# 인쇄 대기열 확인
lpstat -W all
```

Explain results to the user in plain Korean.

To delete and reinstall the printer, **MUST ask user confirmation first**: "프린터 설정을 삭제하고 다시 설치하시겠습니까?"

If confirmed:
```bash
lpadmin -x SINDOH_D451
```
Then restart from Step 3.

## Reference

- Sindoh Download Page: https://www.sindoh.com/support/download/d451
- Default Printer IP: 10.90.36.100
- PPD: SINDOH_D451_CM3035
- Protocol: LPD
- D2SF Setup Guide: https://www.notion.so/d2sf/D2SF-66394477b8704ccc8febd2ae99fa8c77
