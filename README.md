# Active Directory Pentesting Attack Flow

```text
                                      ┌─────────────────────────────┐
                                      │ Initial Enumeration         │
                                      │ • TCP/UDP Scan              │
                                      │ • SMB/RPC/LDAP Enum         │
                                      │ • Anonymous Access          │
                                      │ • Public Shares             │
                                      └──────────────┬──────────────┘
                                                     │
                                                     ▼
                                      ┌─────────────────────────────┐
                                      │ Domain User Enumeration     │
                                      │ • RID Brute                 │
                                      │ • LDAP Users                │
                                      │ • Kerbrute                  │
                                      │ • User Descriptions         │
                                      └──────────────┬──────────────┘
                                                     │
                                                     ▼
                                      ┌─────────────────────────────┐
                                      │ Kerberos-Based Attacks      │
                                      │ • AS-REP Roasting           │
                                      │ • Kerberoasting             │
                                      │ • Kerberos Downgrade        │
                                      └──────────────┬──────────────┘
                                                     │
                                  ┌──────────────────┴──────────────────┐
                                  │                                     │
                                  ▼                                     ▼
                      Hash Cracked?                           Can't Crack Hash?
                            │                                       │
                    ┌───────┴────────┐                              │
                    │                │                              │
                   Yes               No                             │
                    │                │                              │
                    ▼                ▼                              ▼
          Credential Validation    Continue Enumeration      Compromise Another Host
                    │                                             │
                    ▼                                             ▼
      ┌─────────────────────────────┐              ┌─────────────────────────────┐
      │ Credential Validation       │              │ Dump Kerberos Tickets       │
      │ • SMB                       │              │ • Mimikatz                  │
      │ • WinRM                     │              │ • Rubeus                    │
      │ • LDAP                      │              │ • klist                     │
      │ • MSSQL                     │              └──────────────┬──────────────┘
      │ • SSH                       │                             │
      │ • RDP                       │                             ▼
      └──────────────┬──────────────┘                 ┌──────────────────────────┐
                     │                                │ Pass-the-Ticket (PTT)    │
                     ▼                                └──────────────┬───────────┘
      ┌─────────────────────────────┐                             │
      │ SMB Enumeration             │                             │
      │ • Shares                    │                             │
      │ • GPP                       │                             │
      │ • Writable Shares           │                             │
      │ • NTLM Capture              │                             │
      └──────────────┬──────────────┘                             │
                     │                                            │
                     ▼                                            │
      ┌─────────────────────────────┐                             │
      │ Interactive Shell Access    │◄────────────────────────────┘
      │ • WinRM                     │
      │ • RDP                       │
      │ • PsExec                    │
      │ • WMI                       │
      │ • runascs                   │
      └──────────────┬──────────────┘
                     │
                     ▼
      ┌─────────────────────────────┐
      │ Windows Post-Exploitation   │
      │ • WinPEAS                   │
      │ • Credential Hunting        │
      │ • Browser Credentials       │
      │ • PowerShell History        │
      │ • Registry                  │
      │ • Config Files              │
      └──────────────┬──────────────┘
                     │
                     ▼
      ┌─────────────────────────────┐
      │ Local Administrator         │
      │ Post-Exploitation           │
      │ • Mimikatz                  │
      │ • DPAPI                     │
      │ • LSA Secrets               │
      │ • SAM                       │
      │ • NTDS (DC)                 │
      │ • SharpHound                │
      └──────────────┬──────────────┘
                     │
                     ▼
      ┌─────────────────────────────┐
      │ Credential Abuse &          │
      │ Lateral Movement            │
      │ • Pass-the-Hash             │
      │ • Pass-the-Ticket           │
      │ • Overpass-the-Hash         │
      │ • WinRM                     │
      │ • PsExec                    │
      │ • WMIExec                   │
      │ • SMBExec                   │
      └──────────────┬──────────────┘
                     │
                     ▼
      ┌─────────────────────────────┐
      │ Active Directory            │
      │ Enumeration                 │
      │ • BloodHound                │
      │ • ACLs                      │
      │ • GPOs                      │
      │ • Trusts                    │
      │ • Delegation                │
      │ • AD CS                     │
      │ • Sessions                  │
      └──────────────┬──────────────┘
                     │
                     ▼
            High Privileges Obtained?
                     │
         ┌───────────┴────────────┐
         │                        │
         ▼                        ▼
 Service Account Key         Replication Rights
 (NTLM/AES)                  (DCSync)
         │                        │
         ▼                        ▼
 ┌───────────────────┐   ┌────────────────────┐
 │ Silver Ticket     │   │ Dump KRBTGT Hash   │
 └─────────┬─────────┘   └──────────┬─────────┘
           │                        │
           ▼                        ▼
     Service Access         Golden Ticket
           │                        │
           └──────────────┬─────────┘
                          ▼
          ┌─────────────────────────────┐
          │ Domain Persistence          │
          │ • Golden Ticket             │
          │ • Silver Ticket             │
          │ • RBCD                      │
          │ • Shadow Credentials        │
          │ • AD CS                     │
          │ • SID History               │
          └─────────────────────────────┘
```

## Typical Attack Progression

```text
Recon
   │
   ▼
User Enumeration
   │
   ▼
Credential Discovery
   │
   ▼
Initial Foothold
   │
   ▼
Local Enumeration
   │
   ▼
Privilege Escalation
   │
   ▼
Credential Harvesting
   │
   ▼
Lateral Movement
   │
   ▼
Active Directory Enumeration
   │
   ▼
Domain Administrator
   │
   ▼
Kerberos Abuse
   │
   ▼
Persistence
```
