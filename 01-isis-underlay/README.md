[Back to Main Repository](../README.md)

# IS-IS Underlay

A basic two-spine, two-leaf IP fabric using IS-IS as the link-state IGP underlay to provide loopback-to-loopback reachability

## Topology

* **Nodes:** 2 Spines, 2 Leaves
* **Network Type:** Point-to-Point interfaces
* **Addressing:** Unnumbered IP / '/31' point-topoints, '/32' Loopbacks (System IDs derived from Loopbacks)
* **ISIS Area**: '49.0001' (Single-Area, Level-2 only)
* **Vendor-OS:** Arista-EOS

## Objective
Establish L2-free, deterministic IP reachability between all node Loopback0 addresses using IS-IS over Point-to-Point links.

## Design Decisions
* **Level-2 Only:** Configured interfaces as Level-2 to avoid unnecessary L1 database overhead and multiple adjacencies
* **Point-to-Point Network Type** Eliminates DIS election, as modern networks do not share collision domains between devices.
* **Passive Interfaces:** Set Loopback0 as passive to originate the prefix without forming adjacencies or sending IS-IS packets

## Verification Commands

### 1. Adjacencies
Verify all links are UP in Level-2:
```
show isis neighbors
```
### 2. Link-State Database (LSDB)
Confirm all System IDs are present in the L2 database:
```
show isis database
```

### 3. Route Table
Verify Loopback0 reachability across all spine and leaf switches:
```
show ip route isis
```

---

## IP Addressing

### Allocation
| Purpose | Subnet |
| :--- | :--- |
| Leaf Loopbacks | 10.100.0.0/24 |
| Spine Loopbacks | 10.100.1.0/24 |
| Leaf/Spine Connections | 10.100.2.0/23 |

### Interface IP Addresses
| Device | Interface | IP Address |
| :--- | :--- | :--- |
| spine1 | Eth1 | 10.100.2.0/31 |
| spine1 | Eth2 | 10.100.2.2/31 |
| spine1 | Lo0 | 10.100.1.1/32 |
| spine2 | Eth1 | 10.100.2.4/31 |
| spine2 | Eth2 | 10.100.2.6/31 |
| spine2 | Lo0 | 10.100.1.2/32 |
| leaf1 | Eth1 | 10.100.2.1/31 |
| leaf1 | Eth2 | 10.100.2.5/31 |
| leaf1 | Lo0 | 10.100.0.1/32 |
| leaf2 | Eth1 | 10.100.2.3/31 |
| leaf2 | Eth2 | 10.100.2.7/31 |
| leaf2 | Lo0 | 10.100.0.2/32 |