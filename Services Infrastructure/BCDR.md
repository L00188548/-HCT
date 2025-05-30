# BCDR.md

## 1. Backup Strategy

A robust, multi-layered backup strategy will protect all VMs and their data:

- **Daily incremental** and **weekly full** backups using Windows Server Backup or Veeam Community Edition
- Stored on a secure NAS share with restricted access
- Retention policy: 30 days for incrementals; full backups archived monthly to external USB drives
- Weekly verification to test backup integrity and alert on any issues

## 2. Disaster Recovery

To ensure service restoration during critical events:

- **Hyper-V Replica** replicates VMs from Host A to Host B every 5 minutes
- **VM exports** taken monthly and stored offline for cold recovery
- Disaster recovery drills conducted biannually to validate our recovery plan

## 3. Business Continuity

We’ve taken steps to ensure continued service delivery:

- High availability and replication prevent extended downtime
- UPS-protected infrastructure allows time for graceful shutdowns
- Remote access via Windows Admin Center means staff can manage recovery remotely
- Documented runbooks outline step-by-step recovery for each major service

## Justification

Simplicity and dependability come together with this approach. With the hyper-v and windows server tools and current NAS architecture, we are able to achieve enterprise level resiliency without significant investment. It is an optimal solution for small and mid-sized IT ecosystems with critical uptime requirements.
