### Hashtype
```bash
echo -n '$2y$10$iOrk210RQSAzNCx6Vyq2X.aJ/D.GuE4jRIikYiWrD3TM/PjDnXm4q' > hash.txt
```
```bash
hashcat --identify hash.txt
```
![image](https://github.com/user-attachments/assets/c3e69393-9069-48e6-84bc-4db130f75f1b)

### crack
```bash
hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```
- -m hashtype
- -a 0 辞書攻撃

### Show
```bash
hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt --show
```
![image](https://github.com/user-attachments/assets/eb2163e7-7179-4dc1-9313-cc16c589da83)

- --show
