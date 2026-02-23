---
description: This skill should be used when the user asks to "프린터 설치", "복합기 설치", "신도리코 설치", "printer setup", "sindoh setup", "D451 설치", "프린터 추가", "복합기 드라이버", or needs help installing a Sindoh D451 multifunction printer on macOS.
---

# Sindoh D451 Printer Setup

Install the Sindoh D451 (CM3035) multifunction printer on macOS. Follow these steps in order.

## Step 1: Check Current State

Run these diagnostic commands to understand the current environment:

```bash
# macOS 버전 확인
sw_vers

# 기존 설치된 프린터 확인
lpstat -a 2>/dev/null || echo "프린터 없음"

# 신도리코 드라이버 설치 여부 확인
lpinfo -m 2>/dev/null | grep -i 'sindoh.*451\|sindoh.*CM3035'

# 프린터 네트워크 연결 확인
ping -c 2 -W 2 10.90.36.100 2>/dev/null
```

## Step 2: Driver Installation

Check if the D451 PPD is already available:

```bash
lpinfo -m 2>/dev/null | grep -i 'sindoh.*451\|sindoh.*CM3035'
```

**If D451 PPD is NOT found**, guide the user to download the driver:

1. Open the Sindoh download page in a browser: https://www.sindoh.com/support/download/d451
2. Select **Mac OS 15.X** (or matching version from `sw_vers`)
3. Download and install the **SINDOH_D450_CM.PKG** file
4. After installation, verify the PPD is available:
   ```bash
   lpinfo -m 2>/dev/null | grep -i 'sindoh.*451\|sindoh.*CM3035'
   ```

Expected output: `Library/Printers/PPDs/Contents/Resources/SINDOH_D451_CM3035.gz SINDOH D451/CM3035 PS`

**IMPORTANT**: The download page loads dynamically via JavaScript. Direct API access is blocked by Cloudflare. The user MUST use a browser to download.

## Step 3: Add Printer

Add the printer using `lpadmin`:

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

```bash
lpoptions -d SINDOH_D451
```

## Step 5: Test Print

Print a test file to verify the setup:

```bash
# 테스트 페이지 인쇄
lp -d SINDOH_D451 /path/to/test-file.pdf
```

Check print job status:

```bash
lpstat -W all | grep SINDOH_D451
```

## Troubleshooting

If the printer doesn't respond:

```bash
# 네트워크 연결 확인
ping -c 3 10.90.36.100

# 프린터 웹 인터페이스 확인
curl -s --connect-timeout 5 http://10.90.36.100/ | head -20

# SNMP로 프린터 모델 확인
snmpget -v1 -c public 10.90.36.100 1.3.6.1.2.1.1.1.0

# 인쇄 대기열 확인
lpstat -W all

# 프린터 삭제 후 재설치
lpadmin -x SINDOH_D451
```

## Reference

- Sindoh Download Page: https://www.sindoh.com/support/download/d451
- Printer IP: 10.90.36.100
- PPD: SINDOH_D451_CM3035
- Protocol: LPD
- D2SF Setup Guide: https://www.notion.so/d2sf/D2SF-66394477b8704ccc8febd2ae99fa8c77
