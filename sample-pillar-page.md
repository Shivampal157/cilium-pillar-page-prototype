# 🏛️ Pillar Page Prototype: Understanding Cilium Networking

## 1. The Core Problem: Why is kube-proxy slow?

In traditional Kubernetes networking, packets travel through a complex maze of iptables rules.
Every time a new service is added, thousands of rules are updated, slowing down the system.

---

### 🔴 The "Traffic Hairpin" (Old Way – iptables)

```mermaid
graph TD
    User[User Request] -->|1. Hit Node IP| NIC[Network Interface]
    NIC -->|2. Kernel Interrupt| Kernel
    Kernel -->|3. IPTables Lookup (Slow)| DNAT[DNAT Rules]
    DNAT -->|4. Packet Copy| PodA[Target Pod]
sequenceDiagram
    participant Packet
    participant NIC as Network Interface
    participant BPF as Cilium eBPF
    participant Pod as Application Pod

    Packet->>NIC: Arrives at Node
    NIC->>BPF: Intercepted via XDP Hook
    Note over BPF: No iptables lookup<br/>No context switch
    BPF->>Pod: Direct socket redirect
    Pod->>Packet: Response

    style DNAT fill:#ff9999,stroke:#333,stroke-width:2px
