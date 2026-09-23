# Smart Bangladesh Digital City Network

## 1 Introduction

The Government of Bangladesh has launched the Smart Bangladesh Digital City (SBDC) initiative to integrate essential public services into one secure network infrastructure.

You have been appointed as the Lead Network Engineer responsible for designing and implementing the complete network infrastructure connecting six major government departments.

For simplicity, the network consists of the following departments:

| Department                         | Estimated Hosts |
| :--------------------------------- | :-------------- |
| Smart City Control Center (SCCC)   | 320             |
| Traffic Management Authority (TMA) | 180             |
| Public Health Department (PHD)     | 220             |
| Education Service Center (ESC)     | 140             |
| Emergency Response Unit (ERU)      | 100             |
| Digital Citizen Service (DCS)      | 260             |

---

## 2 Requirements

### 2.1 Topology Design

Design the topology according to the following conditions:

1. **Router Representation:** Each department must be represented by one router.
2. **Department Devices:** Every department must contain:
   - **a)** One LAN switch
   - **b)** Two PCs representing all hosts
   - **c)** One Network Printer
3. **Interconnection Requirements:**
   - **a)** SCCC, TMA, and PHD must be connected through one central switch.
   - **b)** ESC must be connected only to DCS.
   - **c)** SCCC must have a direct serial connection with ERU.
   - **d)** ERU must have a direct connection with DCS.
   - **e)** DCS must have a direct connection with TMA.
   - **f)** PHD must have a direct connection with TMA.
4. **Redundancy:** The topology must provide multiple routing paths between departments.

---

### 2.2 Server and Service Configuration

1. **DNS Server**
   - **a)** Only the Smart City Control Center hosts the central DNS server.
   - **b)** All departments must use this DNS server.
2. **Web Servers**
   - **a)** SCCC hosts the website: `www.smartcity.gov.bd`
   - **b)** ERU hosts the website: `www.emergency.gov.bd`
   - **c)** Every PC in the network must be able to access both websites using domain names.
3. **Email Servers**
   - **a)** Only TMA and ESC contain Email Servers.
   - **b)** Domains:
     - **i.** `mail.tma.gov.bd`
     - **ii.** `mail.esc.gov.bd`
   - **c)** Each Email Server must contain at least two user accounts.
   - **d)** Example Accounts:
     - **i.** `traffic1@tma.gov.bd`
     - **ii.** `admin@tma.gov.bd`
     - **iii.** `student@esc.gov.bd`
     - **iv.** `teacher@esc.gov.bd`
   - **e)** Users from both domains must be able to exchange emails successfully.

---

### 2.3 DHCP Configuration

1. All PCs must obtain IP addresses dynamically.
2. **Router-Based DHCP**
   - **a)** The SCCC Router must act as the DHCP Server for:
     - **i.** SCCC
     - **ii.** TMA
     - **iii.** PHD
3. **Server-Based DHCP**
   - **a)** DCS must contain a dedicated DHCP Server.
   - **b)** ERU must contain a dedicated DHCP Server.
4. **DHCP Relay**
   - **a)** ESC does not have its own DHCP Server.
   - **b)** ESC must obtain IP addresses from the DCS DHCP Server.
   - **c)** Configure the required DHCP Relay.

---

### 2.4 Addressing

1. Generate the base network using the first group member’s Student ID.
2. Take the last four digits of the Student ID.
3. Divide them into two groups of two digits.
4. **Given:**
   - Student ID: `22201108`
   - Last four digits: `1108`
   - Base Network: `11.08.0.0/16`
5. If the first pair becomes `00`, use `10` instead.
6. Perform VLSM subnetting for:
   - **a)** Department LANs
   - **b)** Router-to-router links
   - **c)** Server networks (if separated)
7. Routers, printers, and servers must use static IP addresses.
8. PCs must receive IP addresses dynamically.

---

### 2.5 Routing

1. **Dynamic Routing**
   - **a)** SCCC, TMA, and PHD must participate in RIPv2.
   - **b)** They must exchange routing information dynamically.
2. **Hybrid Routing**
   - **a)** The SCCC Router must use both Dynamic Routing and Static Routing.
   - **b)** This router must know every network in the topology.
3. **Static Routing**
   - **a) ERU Router:**
     - **i.** Configure Next-Hop Static Routes to all remote LANs.
   - **b) DCS Router:**
     - **i.** Configure Directly Connected Static Routes.
     - **ii.** Do not configure Fully Specified Static Routes.
   - **c) ESC Router:**
     - **i.** Configure one Default Static Route toward DCS.
   - **d) TMA Router:**
     - **i.** Configure a Floating Static Route through DCS.
     - **ii.** The floating route must become active if the primary path through SCCC fails.

---

### 2.6 Backup Routing

1. During normal operation, TMA must reach all external networks through SCCC.
2. If the SCCC–TMA connection fails, traffic must automatically travel through:
   `TMA -> DCS -> ERU -> SCCC`
   using the configured Floating Static Route.
3. Demonstrate the successful failover.

---

## 3 Deliverables

Submit the following:

1. Cisco Packet Tracer (`.pkt` / `.pka`)
2. Properly labeled topology
3. VLSM calculation
4. IP Address Table
5. DHCP configurations
6. DNS configuration
7. Email Server configuration
8. Static Routing configuration
9. Dynamic Routing configuration
10. Screenshots showing:
    - **a)** Cross-domain email communication
    - **b)** Website access using DNS
    - **c)** DHCP address assignment
    - **d)** Backup route after primary link failure
    - **e)** Successful ping tests between all departments
