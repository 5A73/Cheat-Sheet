
# Install-ADDSForest コマンドの解説

`Install-ADDSForest` は Active Directory ドメインサービス (AD DS) フォレストを新規に作成するための PowerShell コマンドです。このコマンドには、いくつかのオプションが指定されており、具体的な内容は以下の通りです。

```powershell
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\\Windows\\NTDS" -DomainMode "7" -DomainName "cs.org" -DomainNetbiosName "cs" -ForestMode "7" -InstallDns:$true -LogPath "C:\\Windows\\NTDS" -NoRebootOnCompletion:$false -SysvolPath "C:\\Windows\\SYSVOL" -Force:$true
```

## 各パラメータの説明

- **`-CreateDnsDelegation:$false`**  
  DNS デリゲーションを作成しないように設定します。DNS デリゲーションは、フォレストの名前空間に他の DNS サーバーが管理するゾーンがある場合に使用されますが、今回はそれを作成しません。

- **`-DatabasePath "C:\\Windows\\NTDS"`**  
  Active Directory データベースファイル (`ntds.dit`) を保存するパスを指定しています。デフォルトでは `C:\Windows\NTDS` フォルダーにデータベースが保存されます。

- **`-DomainMode "7"`**  
  ドメインの機能レベルを指定します。`7` は Windows Server 2016 のドメイン機能レベルを意味します。これにより、ドメイン内で使用できる機能が Windows Server 2016 に対応したものになります。

- **`-DomainName "cs.org"`**  
  新しく作成するドメインの完全修飾ドメイン名 (FQDN) を指定します。この場合、ドメイン名は `cs.org` です。

- **`-DomainNetbiosName "cs"`**  
  NetBIOS 名を指定します。これは、レガシーなシステムで使用される短い名前で、ここでは `cs` が指定されています。

- **`-ForestMode "7"`**  
  フォレストの機能レベルを指定します。`7` は Windows Server 2016 のフォレスト機能レベルです。

- **`-InstallDns:$true`**  
  DNS サーバーをインストールするかどうかを指定します。このオプションでは、DNS サーバーをインストールする設定になっています。

- **`-LogPath "C:\\Windows\\NTDS"`**  
  Active Directory の操作ログを保存するパスを指定します。ここではデフォルトの `C:\Windows\NTDS` フォルダーが指定されています。

- **`-NoRebootOnCompletion:$false`**  
  インストールが完了した後、自動的に再起動を行うかどうかを指定します。`$false` に設定されているため、インストール後にサーバーは再起動されます。

- **`-SysvolPath "C:\\Windows\\SYSVOL"`**  
  SYSVOL フォルダーの保存パスを指定します。SYSVOL フォルダーにはドメイン内のグループポリシーやスクリプトが格納されます。

- **`-Force:$true`**  
  ユーザーの確認なしに処理を強制的に実行します。

## 全体の解説

このコマンドは、新しい AD DS フォレストを作成し、`cs.org` ドメインを設定します。DNS サーバーもインストールされ、データベースおよびログファイルは `C:\Windows\NTDS` に保存されます。また、フォレストおよびドメインの機能レベルは Windows Server 2016 に設定され、インストールが完了するとサーバーは再起動されます。