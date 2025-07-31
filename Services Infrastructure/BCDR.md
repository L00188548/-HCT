# Electric Petrol's backup, disaster recovery, and business continuity plan
This document explains the backup, disaster recovery (DR), and business continuity (BC) plan for Electric Petrol's Hyper-V environment, which has two servers and is housed at Ireland West Airport (co-location) and Malin, Co Donegal (wholly owned). The plan makes sure that centralised line-of-business apps that support 60 or more service stations, 10 data centre staff, and the Skerries head office can keep working even if something goes wrong. It does this by separating the apps by location to improve continuity.
Plan for Backups


## 1. Plan for Backups

Veeam Backup & Replication Enterprise Edition is a good way to back up your data.

**Configuration:**
- Back up all VMs (DC1, DC2, and application VMs) every day at 1 AM.
- Every Saturday, there is a full backup that is kept on-site at each data centre.
- Ireland West: A 20 TB NAS (QNAP TS-h1886XU-RP) with RAID 6.
- Malin: 20 TB NAS (Synology RS4021xs+) with RAID 6.
- Retention: 14 daily backups, 8 weekly backups, and 3 monthly backups (GFS policy).
- AES-256 encryption for backups.

**Justification:** 
- Veeam Enterprise Edition can back up and replicate data across several sites, making it perfect for data centres that are spread out.
- A large NAS may hold application VMs and grow as needed.
- RAID 6 makes ensuring that local storage is redundant.
- Encryption keeps private information safe, such client transactions.

**Goals for Recovery:**
- RPO: less than 24 hours (data loss is restricted to one day).
- RTO: less than 2 hours (restoring a VM from a local NAS).


## 2. Plan for Disaster Recovery

In a disaster recovery scenario, one data centre could be lost (for example, due to a power failure or fire) or both could be lost (due to a catastrophic event).
**DR Solution:** 
- Backups to Azure Blob Storage in Ireland that are stored off-site. Incremental backups are synced every day, and full backups are synced every week.
- Hyper-V Replica for important VMs (DCs, application servers) between Malin and Ireland West (sync every 5 minutes).
- HPE ProLiant DL360 Gen10 spare parts are kept at the Skerries head office in case of an emergency rebuild.

**Recovery Process:**
- Failover important VMs to the site that is still up and running using Hyper-V Replica.
- Restore VMs that aren't replicated from Azure Blob Storage to a surviving host or spare hardware.
- If necessary, change the configuration of AD sites and services and promote DC at the surviving site to major FSMO roles.

**Justification:** 
- The sites are far apart (Ireland West to Malin, around 200 km), which lowers the danger of both sites being down at the same time.
- Hyper-V Replica makes guarantee that important services can fail over almost instantly.
- For backups that are stored off-site, Azure Blob Storage is affordable and meets regional standards.
- If both locations are affected, spare hardware at Skerries makes recovery quick.

**Goals for Recovery:**
- RPO: less than 5 minutes for replicated VMs and less than 24 hours for other VMs.
- RTO: less than 1 hour for replicated VMs and less than 12 hours for complete site restoration.

## 3. Business Continuity

**High Availability:**
- Hyper-V Replica makes sure that important VMs (including AD, DNS, DHCP, NTP, and apps) are always available at the other site.
- Having extra NICs, power supply, and green energy sources helps keep hardware and electricity from going down.

**Service Redundancy:** 
- Two DCs with AD, DNS, DHCP, and NTP make sure that services are always available across sites.
- DHCP failover (hot standby) and AD replication keep services from going down.

**Operational Continuity:** 
- 10 people at Ireland West learnt how to use Veeam, Hyper-V Replica, and DC recovery.
- The runbook for failover, restoration, and disaster recovery activities is kept in Skerries and both data centres.
- Testing DR methods twice a year to make sure they work.
- The Skerries head office has a VPN connection to both data centres so they can control them from afar.

**Justification:**
- Hyper-V Replica and service redundancy make guarantee that service stations and the head office have as little downtime as possible.
- Training staff and using runbooks help a small team recover with fewer mistakes.
- Testing twice a year strikes a balance between expense and readiness.
- VPN lets Skerries control everything from one place.

**Goals for Continuity:**
- Uptime: 99.95% (very little downtime for important apps).
- Recovery Confidence: Proven through DR tests every six months.

## Justification

- Geographic Separation: The distance between Ireland West and Malin sites helps with disaster recovery (DR) by lowering the likelihood of localised disasters.
- Cost-Effectiveness: Veeam Enterprise and Azure Blob offer enterprise-level features at a price that works for a growing organisation.
- Simplicity: Hyper-V Replica and local NAS backups make things easier for a small IT crew.
- Scalability: Can add more VMs for future service stations or apps.
- Risk Mitigation: Replication, off-site backups, spare hardware, and training help with risks related to hardware, data loss, and operations.
