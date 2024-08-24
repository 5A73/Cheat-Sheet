## nmap
### ホスト探索
#### pingスキャン
```bash
nmap -sn 10.10.11.1/24
```
```bash
nmap -sn 192.168.0.101-150
```
### サービス調査
#### オープンポート調査
```bash
nmap -p- -T4 192.168.56.101
```
```bash
nmap -p- -sV -sC -v 192.168.56.101 --open
```
スキャンで何も得られない場合は、-Pnオプションをつける

#### サービスの詳細情報
```bash
nmap -sV -sC -p80 192.168.0.101
```
- -sV バージョン検出 
- -sC スクリプトスキャン
  
```bash
nmap -A -p80 192.168.0.101
```
- -A OS検出、バージョン検出、スクリプトスキャン、traceroute
#### サービスの既知の脆弱性
```bash
nmap --script  vuln -p80 192.168.0.101
```
#### 1秒あたりの最低パケット数の指定
```bash
nmap -p- --min-rate=10000 10.10.11.249
```


---
## rustscan
#### install
```bash
wget https://github.com/RustScan/RustScan/releases/download/2.0.1/rustscan_2.0.1_amd64.deb
dpkg -i rustscan_2.0.1_amd64.deb
```
#### scan
```bash
rustscan -r 1-65535 -a 10.10.11.248 -- -sV -Pn -A
```
---
## masscan

---
## Bash
```bash
for ip in $(seq 1 254); do ping -c 1 192.168.56.$ip; done
```
---
## PowerShell
```bash
Test-NetConnection -Port <port> <IP>   
```
```bash
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168..0.1", $_)) "TCP port $_ is open"} 2>$null
```
---





