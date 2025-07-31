# Overview of Host Configuration for Electric Petrol Hyper-V Environment
This document explains how to set up two servers for Electric Petrol in two data centres, including the physical setup, the host operating system, high availability, licensing, and operational issues. One server is located at the Ireland West Airport Business Park co-location facility, and the other is located at the company's own greenfield site in Malin, Co Donegal. This is to handle centralised line-of-business applications for more than 60 service stations, 10 data centre staff, and the Skerries head office.


## 1. Physical Setup Hardware Requirements

For this project, we’re setting up two high-performance servers using the **Dell PowerEdge R650** platform ideal for virtualization in a compact 1U form factor. These enterprise grade servers feature:

**2 Dell** PowerEdge R650 servers (1U rack servers)
- Host 1: Ireland West Airport Business Park (multi-tenant, co-location).
- Host 2: Malin, Co Donegal (a greenfield location that is completely owned).

**CPUs:** Each server has two Intel Xeon Gold 6330 CPUs, each with 28 cores and 56 threads.

**RAM:** 256 GB DDR4 ECC (can be expanded to 1 TB).

**Storage:** 
- 2 SSDs with 960 GB each (RAID 1) for the host OS.
- 6 x 3.84 TB NVMe SSD (RAID 10) for storing VMs.

**Networking:** 4 25 GbE NICs (two for VM traffic, one for management, and one for replication).

**Power:** Backup power supplies that are linked to renewable energy sources like wind and solar.

**Justification:**
- CPU and RAM: The Dell PowerEdge R650 with Xeon Gold 6330 processors has a lot of cores and threads, which lets it run several VMs for AD, DNS, DHCP, NTP, and line-of-business apps (including invoicing and inventory) at more than 60 service stations.
- **Storage:** RAID 1 makes sure the OS is reliable, while RAID 10 with 3.84 TB NVMe SSDs gives you high-performance, high-capacity VM storage for important apps.
- **Network:** 25 GbE NICs manage a lot of traffic from service stations and replication between faraway sites, making sure that latency is low and throughput is high.
- **Power:** Electric Petrol's sustainability goals are in line with having extra power supplies that use green energy.
- **Loaction:** Being about 200 miles apart helps with disaster recovery.



## 2. Operating System for the Host

Each host will run **Windows Server 2022 Datacenter Edition**, is the operating system.

### Why This OS?
- Datacenter Edition: Can handle as many VMs as you want, making it great for centralised apps.
- Hyper-V Role: Offers strong virtualisation with tools for replication and management.
- Support: Long-term support till 2032 guarantees that business apps will work well.

## 3. High Availability
#### Hyper-V Replica:
To ensure continuous service delivery, high availability is built into every layer:

- Host 1 (Ireland West) and Host 2 (Malin) replicate data in an asynchronous way.
- Every five minutes, VMs on Host 1 and Host 2 sync with each other.
- Failover can be started by hand or by scripts if a host fails.
- Redundant networking and power (dual NICs, dual PSUs) add fault tolerance
#### Network Redundancy:
- NIC teaming (LACP) for management interfaces and VM traffic.
- A dedicated VPN tunnel over 25 GbE for copying data between sites.
- Management, VM traffic, and replication all have their own VLANs.
#### Justification:
- Hyper-V Replica makes sure that VMs may switch to the other location, which reduces downtime.
- VPN tunnels protect replication between sites over public or shared networks.
- Network redundancy stops interruptions that affect connectivity.

This configuration ensures minimal disruption even during maintenance or unexpected outages.

## 4. Getting a licence
**Windows Server 2022 Datacenter:** Each server needs 56 core licenses, one for each core.
Includes infinite VMs that can run both current and future apps.

**Licenses for Client Access (CALs):**
100 User CALs for AD authentication (for 10 people who work in data centres, the Skerries head office, and service station managers).
200 Device CALs for DHCP clients, which include service station devices like POS systems and IoT sensors.

**Justification:** 
- Datacenter licensing is a good deal for several VMs on two sites.
- CALs make guarantee that users and devices that use centralised services follow the rules.

## 4. Operations
- Windows Admin Centre is a management tool that lets you manage Hyper-V hosts and VMs from a distance.
PowerShell scripts that automatically set up VMs and keep an eye on their replication.

- **Monitoring:** Azure Monitor (via a VPN) lets you see how well your hosts and VMs are doing in real time.
Alerts for CPU usage over 80%, RAM usage over 90%, disc space less than 10% free, and replication delays over 10 minutes.

- **Patching:** Windows Server Update Services (WSUS) sends out updates every month.
To keep things available, patching is done in stages: first Ireland West, then Malin.

- **Justification:** Windows Admin Centre lets you handle things from afar, which is very important for sites that are far apart.
Azure Monitor gives you cloud-based information without needing any on-premises hardware.
Staggered patching makes ensuring that one site stays up and running while changes are being made.



