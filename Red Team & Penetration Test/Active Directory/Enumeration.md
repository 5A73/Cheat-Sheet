## 目次

1. [Enumeration](#enumeration)
2. [PowerView](#powerview)
3. [Bloodhound](#bloodhound)
   - [Collection Methods - Database](#collection-methods---database)
   - [Running Bloodhound](#running-bloodhound)
4. [PsLoggedon](#psloggedon)

---

## Enumeration

To check local administrators in a domain-joined machine:

```bash
net localgroup Administrators
```

---

## PowerView

```powershell
Import-Module .\PowerView.ps1 # Load module to PowerShell. If it gives an error, change the execution policy.
Get-NetDomain # Basic information about the domain.
Get-NetUser # List all users in the domain.
```

- The above command's outputs can be filtered using the `select` command. For example, `Get-NetUser | select cn`, where `cn` is a field in the output of the above command. You can select any number of them separated by commas.

```powershell
Get-NetGroup # Enumerate domain groups.
Get-NetGroup "group name" # Information from a specific group.
Get-NetComputer # Enumerate the computer objects in the domain.
Find-LocalAdminAccess # Scan the network to determine if the current user has administrative permissions on any computers in the domain.
Get-NetSession -ComputerName files04 -Verbose # Check logged on users with Get-NetSession. Adding verbosity gives more info.
Get-NetUser -SPN | select samaccountname,serviceprincipalname # List SPN accounts in the domain.
Get-ObjectAcl -Identity <user> # Enumerate ACE (Access Control Entities), lists SID (Security Identifier), ObjectSID.
Convert-SidToName <sid/objsid> # Convert SID/ObjSID to a name.
```

- Checking for "GenericAll" rights for a specific group. After obtaining them, they can be converted using `Convert-SidToName`:

```powershell
Get-ObjectAcl -Identity "group-name" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights 
```

```powershell
Find-DomainShare # Find the shares in the domain.
Get-DomainUser -PreauthNotRequired -verbose # Identify AS-REP roastable accounts.
Get-NetUser -SPN | select serviceprincipalname # List Kerberoastable accounts.
```

---

## Bloodhound

### Collection Methods - Database

```powershell
# Sharphound - Transfer sharphound.ps1 into the compromised machine.
Import-Module .\Sharphound.ps1 
Invoke-BloodHound -CollectionMethod All -OutputDirectory <location> -OutputPrefix "name" # Collects data and saves with the specified details. Output will be saved in the compromised machine.
```

```bash
# Bloodhound-Python
bloodhound-python -u 'uname' -p 'pass' -ns <rhost> -d <domain-name> -c all # Output will be saved on your Kali machine.
```

### Running Bloodhound

```bash
sudo neo4j console
# Then upload the .json files obtained.
```

---

## PsLoggedon

To see user logons at a remote system of a domain (external tool):

```bash
.\PsLoggedon.exe \\<computername>
```
