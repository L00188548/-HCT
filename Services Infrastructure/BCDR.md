# BCDR.md
This document details the Backup, Disaster Recovery and Business Continuity (BCDR) practices for a small business’s critical services supported by a two-server Hyper-V environment. The strategy employs backup infrastructure to ensure low-cost recovery while maintaining effective business continuity capabilities.


## 1. Backup Strategy

A robust, multi-layered backup strategy will protect all VMs and their data:

- **Daily incremental** and **weekly full** backups using Windows Server Backup or Veeam Community Edition
- Stored on a secure NAS share with restricted access
- **Offsite backups:** Full backups are encrypted and uploaded for example to Azure storage for protection against disasters.
- **Retention policy:** 30 days for incrementals; full backups archived monthly to external USB drives
- **Verfications:** Weekly verification to test backup integrity and alert on any issues

This approach provides a cost-effective solution while still allowing for regular backups.



## 2. Disaster Recovery

To ensure service restoration during critical events:

- **Hyper-V Replica** replicates VMs from Host A to Host B every 5 minutes
- **VM exports** taken monthly and stored offline for cold recovery
- Disaster recovery drills conducted annually to validate our recovery plan

This approach allows a small business to recover quickly and keep interruptions to authentication and network services to a minimum.



## 3. Business Continuity

We’ve taken steps to ensure continued service delivery:

- High availability and replication prevent extended downtime
- UPS-protected infrastructure allows time for graceful shutdowns
- Remote access via Windows Admin Center means staff can manage recovery remotely
- Documented runbooks outline step by step recovery for each major service

This ensures that the business can operate with minimal interference, even in the event of large scale disruptions.



## Justification

This BCDR strategy is tailored for a small business Simplicity and dependability come together with this approach. With the hyper-v and windows server tools and current NAS architecture, we are able to achieve enterprise level resiliency without significant investment. It is an optimal solution for small and mid-sized IT ecosystems with critical uptime requirements.
