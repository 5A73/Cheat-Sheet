## nmap
### ホスト探索
### ポート列挙

### サービス調査


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





