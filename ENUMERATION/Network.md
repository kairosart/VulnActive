Enumerate the network with `nmap`.

```
sudo nmap -sSVC -p- -oA nmap_full -Pn <MACHINE IP>
```

![[Screenshot from 2025-10-03 19-47-16.png]]

There are several opened ports. For your work the most interesting ports are:

445 SMB
6379 REDIS

**Next step:** [[SMB]]

