# SSH（Secure Shell）概要と使い方

### 概要
SSHは、ネットワーク経由でリモートシステムに安全にアクセスするためのプロトコルです。暗号化された接続を使用して、リモートシェルの取得やコマンドの実行、ファイル転送などを行います。通常、22番ポートを使用します。

### セキュリティ上の注意点
- **強力な認証**: パスワード認証よりも公開鍵認証の使用が推奨されます。公開鍵認証は、より高いセキュリティを提供します。
- **パスワードの複雑さ**: SSHサーバーに強力で複雑なパスワードを設定し、ブルートフォース攻撃を防ぎます。
- **不正アクセスの防止**: 不要なユーザーアカウントやグループの削除、SSH設定の最適化（例: rootログインの無効化）を行います。

## 偵察

#### Nmapを使用したSSHサーバーの偵察
SSHサーバーの詳細情報を取得し、脆弱性を調査するための基本的な手順です。

```bash
# 1. SSHサービスのバージョン検出
nmap -sV -p 22 ssh.example.com

# 2. SSH特有のスクリプトを使用して脆弱性をチェック
nmap --script ssh-hostkey -p 22 ssh.example.com
```

#### SSHブルートフォース攻撃
SSHサーバーに対してブルートフォース攻撃を行い、認証情報を試す方法です。

```bash
# Hydraを使用したSSHブルートフォース攻撃
hydra -l username -P /path/to/passwordlist.txt ssh://ssh.example.com
```

- **-l**: 単一のユーザー名を指定します。
- **-P**: パスワードリストファイルを指定します。


#### SSHブルートフォーススクリプト
SSHサーバーに対してブルートフォース攻撃を行うための簡単なスクリプトです。

```bash
# パスワードリストを使用してブルートフォース攻撃
for password in $(cat /path/to/passwordlist.txt); do
    echo "Trying password: $password"
    sshpass -p "$password" ssh -o StrictHostKeyChecking=no user@ssh.example.com exit
    if [ $? -eq 0 ]; then
        echo "Password found: $password"
        break
    fi
done
```

- **sshpass**: パスワードを自動的に提供するツールです。
- **-o StrictHostKeyChecking=no**: ホストキーの確認を無効にします。

#### `ssh-keyscan`ツール
SSHサーバーの公開鍵を収集するためのツールです。サーバーの鍵を検証するのに役立ちます。

```bash
# SSHサーバーの公開鍵を収集
ssh-keyscan ssh.example.com
```

- **ssh-keyscan**: SSHサーバーから公開鍵を取得します。

#### `ssh-keygen`ツール
SSH鍵ペアを生成するためのツールです。公開鍵認証の設定に使用されます。

```bash
# 新しいSSH鍵ペアを生成
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa
```

- **-t rsa**: RSA鍵の生成を指定します。
- **-b 2048**: 鍵のビット数を指定します。
- **-f**: 鍵ファイルの出力先を指定します。

## ログイン

#### SSH接続の確立
SSH接続を確立し、リモートシステムにアクセスします。

```bash
# SSH接続を確立
ssh user@ssh.example.com

# 特定の秘密鍵を使用して接続
ssh -i /path/to/private_key user@ssh.example.com
```

- **user**: SSH接続先のユーザー名を指定します。
- **ssh.example.com**: 接続先のホスト名またはIPアドレスを指定します。
- **-i**: 使用する秘密鍵ファイルを指定します。

## ログイン後

#### SSH設定ファイルの確認
リモートサーバーのSSH設定ファイルを確認し、セキュリティ設定を調査します。

```bash
# SSH設定ファイルの確認
cat /etc/ssh/sshd_config
```

- **/etc/ssh/sshd_config**: SSHデーモンの設定ファイルです。

#### 公開鍵認証の設定
SSH公開鍵認証の設定を行い、認証のセキュリティを強化します。

```bash
# 公開鍵をリモートサーバーにコピー
ssh-copy-id user@ssh.example.com
```

- **ssh-copy-id**: ローカルの公開鍵をリモートサーバーのauthorized_keysに追加します。

#### SSHトンネリングの使用
SSHトンネルを使用して、安全に他のサービスにアクセスします。

```bash
# ローカルポートフォワーディングの設定
ssh -L local_port:remote_host:remote_port user@ssh.example.com

# ダイナミックポートフォワーディングの設定
ssh -D local_port user@ssh.example.com
```

- **-L**: ローカルポートフォワーディングを設定します。
- **-D**: ダイナミックポートフォワーディングを設定します。

### エラー

#### Permission denied, please try again
認証に失敗したことを示します。

- **原因**: ユーザー名やパスワードが間違っている、または公開鍵がサーバーに設定されていない可能性があります。
- **対処法**: 認証情報を再確認し、正しい公開鍵がサーバーに設定されているか確認します。

#### Connection refused
SSHサーバーへの接続が拒否されたことを示します。

- **原因**: SSHサーバーが稼働していない、またはファイアウォールによって接続がブロックされている可能性があります。
- **対処法**: SSHサーバーが稼働しているか確認し、ファイアウォールの設定をチェックします。
```
