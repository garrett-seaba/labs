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