# git
## リポジトリのクローン
```bash
git clone https://github.com/5A73/Cheat-Sheet.git
```
### git log
```bash
git log
```
![image](https://github.com/user-attachments/assets/e8e93304-59b6-4ac6-9315-3c43fc8cc1a2)

## フォルダ名の変更
1. リポジトリのクローン

```bash
sudo git clone https://github. com/5A73/Cheat-Sheet. git

```

2. フォルダの移動
```bash
cd Cheat-Sheet

```
3. フォルダ名の変更

```bash
sudo mv oldname newname

```
4. 変更をステージング
```bash
sudo git add -A

```
5. 変更をコミット

```bash
sudo git commit -m "Renamed Folder"

```
- 必ずなにか変更メモを入力する
6. Githubにプッシュ

```bash
sudo git push origin main

```
