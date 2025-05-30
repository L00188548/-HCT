# ServicesDescription.md

## Overview of Provided Services

This environment will host two key virtual servers, each dedicated to essential IT services:

1. **INF-DC01** — Primary domain controller handling Active Directory, DNS, and authentication
2. **INF-INF01** — Infrastructure support server running DHCP, NTP, and file services

## 1. Active Directory (AD)

- Installed on INF-DC01
- Domain: `corp.local`
- Centralized identity and access management
- Group Policy Objects (GPOs) enforce security, drive mappings, and software deployment

## 2. DNS

- Installed with AD on INF-DC01
- Enables internal hostname resolution and supports dynamic updates
- Forwards external DNS requests to the ISP’s resolver or public DNS (e.g., 8.8.8.8)

## 3. DHCP

- Hosted on INF-INF01
- Scope: 192.168.10.0/24
- Assigns IP addresses dynamically with 8-day leases
- Reservations for critical infrastructure (e.g., NAS, printer)

## 4. NTP (Time Synchronization)

- Windows Time Service (W32Time) active on all servers
- INF-DC01 syncs with an external source like `pool.ntp.org`
- Clients sync time via Group Policy, ensuring log consistency and domain trust

This separation of roles across two VMs helps distribute the load and improve fault tolerance.
