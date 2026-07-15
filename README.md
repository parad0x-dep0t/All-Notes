Kerberoasting
        │
        ▼
Can't crack hash?
        │
        ▼
Compromise another machine
        │
        ▼
Dump Kerberos Tickets
        │
        ▼
Pass-the-Ticket
        │
        ├── Enough privileges?
        │        │
        │        ▼
        │   DCSync → KRBTGT
        │        │
        │        ▼
        │   Golden Ticket
        │
        └── Service account hash?
                 │
                 ▼
           Silver Ticket
