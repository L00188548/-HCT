# HostConfiguration.md

## 1. Physical Configuration of Hosts

For this project, we’re setting up two high-performance servers using the **Dell PowerEdge R650** platform—ideal for virtualization in a compact 1U form factor. These enterprise-grade servers feature:

- Dual Intel Xeon Silver 4314 CPUs (32 total cores per server)
- 256 GB DDR4 ECC memory for high reliability
- 4 × 10TB SATA drives in a fault-tolerant RAID 10 array
- Dual 800W redundant power supplies for uninterrupted power
- Quad 1GbE and dual 10GbE NICs for robust network connectivity

Both servers will be rack-mounted and connected to an **APC Smart-UPS 1500VA** to guard against power interruptions and allow for safe shutdowns if needed.

## 2. Host Operating System

Each host will run **Windows Server 2022 Datacenter Edition**, selected for its full Hyper-V feature set and enterprise scalability.

### Why This OS?
- Built-in Hyper-V role with support for live migration and clustering
- Unlimited guest VM rights under Datacenter licensing
- Advanced storage and network capabilities

Licensing will be managed through Microsoft’s Volume Licensing program or an OEM agreement, depending on the final acquisition strategy.

## 3. High Availability

To ensure continuous service delivery, high availability is built into every layer:

- **Failover Clustering** allows VMs to restart on the secondary host in case of failure
- **Live Migration** enables moving VMs between hosts with zero downtime
- **Shared Storage** (via SMB3/iSCSI on NAS) ensures VM data is available to both hosts
- Redundant networking and power (dual NICs, dual PSUs) add fault tolerance

This configuration ensures minimal disruption even during maintenance or unexpected outages.

## 4. Operations

Day-to-day administration will be handled through **Hyper-V Manager** and **Windows Admin Center**, offering local and remote management capabilities.

Monitoring will be provided by native tools such as **Prometheus** or **Zabbix** for deeper insights. Snapshots and routine backups will be aligned with our BCDR plan to safeguard VM data.
