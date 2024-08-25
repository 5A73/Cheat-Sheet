
# Linux設定およびコマンドガイド

## 目次
1. [設定関連](#設定関連)
   - [日本語化](#日本語化)
   - [ターミナル分割](#ターミナル分割)
     - [Terminator](#Terminator)
     - [tmux](#tmux)
   - [カーネルバージョン変更](#カーネルバージョン変更)
2. [Network](#Network)
   - [iptables](#iptables)
3. [ユーザ](#ユーザ)
   - [Interactive](#Interactive)
   - [UID設定](#UID設定)
4. [認証](#認証)
   - [SSH](#SSH)
5. [表示](#表示)
   - [大文字小文字変換](#大文字小文字変換)
   - [treeコマンド](#treeコマンド)
6. [web関連](#web関連)
   - [wget](#wget)
   - [curl](#curl)
7. [データベース操作](#データベース操作)
   - [mysql](#mysql)
8. [ファイル操作](#ファイル操作)
   - [圧縮・解凍](#圧縮・解凍)
   - [viエディタ](#viエディタ)
   - [ls](#ls)
   - [awk](#awk)
   - [cp](#cp)
   - [find](#find)
   - [grep](#grep)
   - [echo](#echo)
9. [重要ファイル一覧](#重要ファイル一覧)

---

## 設定関連

### 日本語化
- [日本語化手順](https://qiita.com/nm_suisai/items/ee7df3ba45228ebdf2aa)

### ターミナル分割

#### Terminator
- [Terminatorの使用方法](https://qiita.com/Hashibirokou/items/58cfe84975c3b3af0235)

#### tmux
- [アニメーション設定](https://qiita.com/KoyanagiHitoshi/items/318d4b8ef3b4e5b87390)
- [tmuxの使い方](https://qiita.com/shin-ch13/items/9d207a70ccc8467f7bab)

### カーネルバージョン変更

```bash
# 以前のバージョンのカーネルパッケージを検索します
apt search linux-image

# 適切な以前のバージョンのカーネルパッケージをインストールします
apt install linux-image-version

# 新しいカーネルをアンインストールします
apt purge linux-image-<new_version>

# ブートローダーを更新します
update-grub

# システムを再起動します
reboot
```

## Network

### iptables
- [iptablesの使い方1](https://qiita.com/dtanimoto00/items/1d0a9b02867add646ea5)
- [iptablesの使い方2](https://knowledge.sakura.ad.jp/4048/?amp=1)

## ユーザ

### Interactive

```bash
adduser uname
useradd uname
```

### UID設定

```bash
# UIDを指定してユーザを特定のグループに追加します
useradd -u UID -g group uname
```

## 認証

### SSH

#### パスワード認証

```bash
ssh test@192.168.0.1
# パスワードを入力
```

#### ログイン時にrbash（制限付きシェル）を回避する方法

```bash
ssh seppuku@192.168.0.115 -t "bash --noprofile"
```

#### サーバの暗号アルゴリズムに合わせる

```bash
ssh -c <cipher_algorithm> username@hostname
```

## 表示

### 大文字小文字変換

```bash
echo "Hello WoRLd" | tr '[:upper:][:lower:]' '[:lower:][:upper:]'
```

### treeコマンド

```bash
tree -l -L 3 /usr/share/wordlists
```

## web関連

### wget

#### ファイルダウンロード

```bash
wget http://lhost/file
```

### curl

#### ファイルダウンロード

```bash
curl http://<LHOST>/<FILE> > <OUTPUT_FILE>
```

## データベース操作

### mysql
```bash
mysql -u username -p
```
```bash
mysql -h localhost -u username -p
show databases;
use databasename;
show tables;
select from * tablenames;
```

## ファイル操作

### 圧縮・解凍

#### .zip

##### 圧縮

```bash
unzip test.zip
```

##### 解凍

```bash
# .zipの解凍コマンドがありませんでしたので、必要であれば追記してください。
```

#### .gz

##### 圧縮

```bash
# .gzの圧縮コマンドがありませんでしたので、必要であれば追記してください。
```

##### 解凍

```bash
gzip -d file.gz
```

#### .tar.gz

##### 圧縮

```bash
tar -zcvf xxxx.tar.gz directory
```

##### 解凍

```bash
tar -zxvf xxxx.tar.gz
tar -xzvf filename.tar.gz
```

#### .tar.bz2

##### 圧縮

```bash
tar -jcvf xxxx.tar.bz2 directory
```

##### 解凍

```bash
tar -jxvf xxxx.tar.bz2
```

#### 7zip

```bash
# 7zipのコマンドがありませんでしたので、必要であれば追記してください。
```

### viエディタ

#### 置換

```bash
:%s/aaa/bbb/g
```

### ls

#### lsのタイムスタンプを見やすくする

```bash
ls -l --time-style=long-iso
```

### awk
- ログ解析などに有用なコマンド
- awkはスペースでフィールドを区切る
- awk '条件 {処理}'
```bash
awk '/exe/  {print $0}'
```
exeのあるファイルを表示
```bash
awk '{print $6}' wordpress.log | sort | uniq -c
```

### cp

```bash
# cpコマンドでディレクトリ内の中身のみ、他のディレクトリにコピーする 
cp -r dir1/. dir2/
```

### find

```bash
find / -name ファイル名
```

### grep

```bash
grep -r hoge
```

### echo

```bash
# hosts追記
echo "10.10.11.249 play.crafty.htb" | tee -a /etc/hosts
```

## 重要ファイル一覧

```bash
/etc/passwd
/etc/shadow
/etc/aliases
/etc/anacrontab
/etc/apache2/apache2.conf
/etc/apache2/httpd.conf
/etc/apache2/sites-enabled/000-default.conf
/etc/at.allow
/etc/at.deny
/etc/bashrc
/etc/bootptab
/etc/chrootUsers
/etc/chttp.conf
/etc/cron.allow
/etc/cron.deny
/etc/crontab
/etc/cups/cupsd.conf
/etc/exports
/etc/fstab
/etc/ftpaccess
/etc/ftpchroot
/etc/ftphosts
/etc/groups
/etc/grub.conf
/etc/hosts
/etc/hosts.allow
/etc/hosts.deny
/etc/httpd/access.conf
/etc/httpd/conf/httpd.conf
/etc/httpd/httpd.conf
/etc/httpd/logs/access_log
/etc/httpd/logs/access.log
/etc/httpd/logs/error_log
/etc/httpd/logs/error.log
/etc/httpd/php.ini
/etc/httpd/srm.conf
/etc/inetd.conf
/etc/inittab
/etc/issue
/etc/knockd.conf
/etc/lighttpd.conf
/etc/lilo.conf
/etc/logrotate.d/ftp
/etc/logrotate.d/proftpd
/etc/logrotate.d/vsftpd.log
/etc/lsb-release
/etc/motd
/etc/modules.conf
/etc/motd
/etc/mtab
/etc/my.cnf
/etc/my.conf
/etc/mysql/my.cnf
/etc/network/interfaces
/etc/networks
/etc/npasswd
/etc/passwd
/etc/php4.4/fcgi/php.ini
/etc/php4/apache2/php.ini
/etc/php4/apache/php.ini
/etc/php4/cgi/php.ini
/etc/php4/apache2/php.ini
/etc/php5/apache2/php.ini
/etc/php5/apache/php.ini
/etc/php/apache2/php.ini
/etc/php/apache/php.ini
/etc/php/cgi/php.ini
/etc/php.ini
/etc/php/php4/php.ini
/etc/php/php.ini
/etc/printcap
/etc/profile
/etc/proftp.conf
/etc/proftpd/proftpd.conf
/etc/pure-ftpd.conf
/etc/pureftpd.passwd
/etc/pureftpd.pdb
/etc/pure-ftpd/pure-ftpd.conf
/etc/pure-ftpd/pure-ftpd.pdb
/etc/rc.d/rc.local
/etc/resolv.conf
/etc/rinetd.conf
/etc/samba/smb.conf
/etc/securetty
/etc/security/access.conf
/etc/services
/etc/ssh/sshd_config
/etc/ssh/ssh_host_dsa_key
/etc/ssh/ssh_host_rsa_key
/etc/sudoers
/etc/sysconfig/iptables
/etc/sysctl.conf
/etc/syslog.conf
/etc/telnetd.conf
/etc/vimrc
/etc/vsftpd.chroot_list
/etc/vsftpd.conf
/etc/vsftpd.user_list
/etc/wgetrc
/etc/xinetd.conf
/etc/xinetd.d/tftp
/etc/yum.conf
/usr/local/etc/php.ini
```

---

これでマークダウン形式になっています。もし他にも修正や追加が必要であればお知らせください。
