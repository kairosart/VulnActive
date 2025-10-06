The VulnNet: Active TryHackMe room is a medium-difficulty Windows-focused penetration testing challenge centered on Active Directory exploitation, though the target machine does not behave like a typical domain controller despite being part of a domain environment. The objective is to gain full access to the system and compromise the domain.

## Initial Access

Initial access is achieved by exploiting a misconfigured Redis database running on port 6379. By connecting to Redis using `redis-cli`, an attacker can manipulate the `CONFIG SET` command to force the service to attempt authentication against a malicious SMB share hosted on the attacker’s machine, thereby capturing the NTLMv2 hash of the `enterprise-security` user. This hash is then cracked using tools like `john` or `hashcat` with the `rockyou.txt` wordlist, revealing the password `sand_0873959498`.

## SMB Enumeration

With the obtained credentials, the attacker enumerates SMB shares using `smbclient` and discovers a file named `PurgeIrrelevantData_1826.ps1` located in the `Enterprise-Share` share. This file is executed by a scheduled task, and since the share is writable, the attacker replaces the file with a malicious PowerShell script (e.g., `Invoke-PowerShellTcp.ps1`) that establishes a reverse shell back to the attacker’s machine. This provides a foothold as the `enterprise-security` user.

## Privilege Escalation

Privilege escalation is achieved through multiple possible methods. 

- One approach involves exploiting the PrintNightmare vulnerability (CVE-2021-1675) by uploading and executing a malicious DLL via the reverse shell, which adds a new local administrator account (`adm1n` with password `P@ssw0rd`) to the system. 
- Alternatively, attackers can use BloodHound to identify attack paths within the domain. Enumeration reveals that the `enterprise-security` user has `GenericWrite` permissions on a Group Policy Object (GPO) named `SECURITY-POL-VN`. Using a tool like `SharpGPOAbuse`, the attacker can modify the GPO to create a scheduled task that adds the `enterprise-security` user to the local Administrators group. After forcing a group policy update with `gpupdate /force`, the user gains administrative privileges, allowing access to the `system.txt` flag located on the desktop.

The room emphasizes the exploitation of misconfigured services (Redis), weak file permissions on SMB shares, and privilege escalation via unpatched vulnerabilities or misconfigured Active Directory policies. Despite the domain context, the machine does not expose typical domain controller ports, requiring attackers to treat it as a standalone Windows system initially.
