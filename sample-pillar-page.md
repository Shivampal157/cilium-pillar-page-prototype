    # 🏛️ Pillar Page Prototype: Understanding Cilium Networking

> *Note:* This page demonstrates the "Docs-as-Code" approach using Mermaid.js diagrams to explain complex concepts visually.

## 1. The Core Problem: Why is kube-proxy slow?
In traditional Kubernetes networking, packets travel through a complex maze of iptables rules. Every time a new service is added, thousands of rules are updated, slowing down the system.

### 🔴 The "Traffic Hairpin" (Old Way)
Notice how the packet bounces around unnecessarily:

```mermaid
graph TD
    User[User Request] -->|1. Hit Node IP| NIC[Network Interface]
    NIC -->|2. Kernel Interrupt| Kernel
    Kernel -->|3. IPTables Lookup (Slow)| DNAT[DNAT Rules]
    DNAT -->|4. Packet Copy| PodA[Target Pod]
    
    style DNAT fill:#ff9999,stroke:#333,stroke-width:2px
    style IPTables fill:#ff9999,stroke:#333,stroke-width:2px
