# Overview of the Electric Petrol Hyper-V Environment Services
This document explains what the two-server Hyper-V system does for Electric Petrol. It supports centralised line-of-business applications for 60+ service stations, 10 employees at Ireland West Airport, and the Skerries head office. Active Directory (AD), Domain Name System (DNS), Dynamic Host Configuration Protocol (DHCP), and Network Time Protocol (NTP) are all hosted at data centres in Ireland West Airport and Malin, Co Donegal.
Active Directory (AD)

# Overview

## 1. Active Directory (AD)
**Setup:**
- Two virtualised Domain Controllers (DCs) that run Windows Server 2022 Standard.
- Host 1 (Ireland West Airport) has DC1.
- DC2 on Host 2 (Malin).
 - DC1 has the PDC, RID, and Infrastructure responsibilities, while DC2 has the Schema and Domain Naming roles.
- Group Policies for service stations that control user authentication, application access (such inventory and billing), and security settings.
- Subnets have been set up for Ireland West, Malin, Skerries, and more than 60 service stations.

**Specifications:**
- VM Specs: 4 vCPUs, 16 GB of RAM, and 150 GB of VHDX for each DC.
- Replication: AD site connects with 15-minute replication within the same site and 60-minute replication across sites.

**Justification:**
- If one site fails, two DCs in different locations make sure that authentication is still available.
- Splitting FSMO roles improves performance across different sites.
- Site-specific subnets make it easier for clients to authenticate themselves at scattered stations.


## 2. The Domain Name System (DNS)

**Configuration:**
- Both DCs (DC1 and DC2) have AD-integrated DNS.
- The principal zone for the business domain is electricpetrol.ie.
- Senders to external DNS (like 1.1.1.1) for internet resolution.
- DNS records for devices at service stations and applications that are hosted in one place.
- Enabled scavenging (removal of records that are 7 days old).

**Specifications:**
- Shares VM resources with AD (4 vCPUs and 16 GB of RAM).
- Conditional forwarders for interfaces with outside parties, like payment processors.

**Justification:**
- AD-integrated DNS makes guarantee that replication and fault tolerance work across locations.
- External forwarders help service stations get stable internet resolution.
- Scavenging maintains DNS clean in places where things change all the time, like service stations.


## 3. DHCP stands for Dynamic Host Configuration Protocol.

**Configuration:** 
- There are two DHCP servers, one for each DC, that are set up to failover (hot standby mode, DC1 is the primary server and DC2 is the secondary server).
- Scope: 10.0.0.100–10.0.0.250 for each service station subnet (60+ scopes).
- Booking for important devices like point-of-sale systems and electric vehicle chargers.
- Options: DNS servers (DC1 and DC2 IPs) and the default gateway (site-specific).

**Specifications:** 
- It shares VM resources with AD and DNS.
- Lease length: 4 days to allow for changing station devices.

**Justification:** 
- Hot standby mode makes ensuring that DHCP is still available if the main site goes down.
- Per-station scopes let you create separate networks for security and management.
- Reservations keep IP addresses from conflicting with each other for important infrastructure.


## 4. The Network Time Protocol (NTP)

**Configuration:** 
- The NTP service is running on both DCs and is synced with an outside source, like ie.pool.ntp.org.
- DCs act as NTP servers for clients that are part of the domain, like service stations and the head office.
- Group Policy sets up clients to sync with either DC1 (the main one) or DC2 (the backup one).


**Specification:** 
- Shares VM resources with DHCP, AD, and DNS.
- Polling time: every 10 minutes for optimum accuracy.


**Justification:** 
- External NTP sync makes ensuring that AD Kerberos and transaction logging have the right time.
- It is easier to set up dispersed clients when DCs act as NTP servers.
- High-frequency polling works with time-sensitive apps like billing.

