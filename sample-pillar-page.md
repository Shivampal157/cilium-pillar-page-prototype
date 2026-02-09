# 🏛️ Pillar Page Prototype: Understanding Cilium Networking

> *Note:* This page demonstrates the "Docs-as-Code" approach using Mermaid.js diagrams.

## 1. The Core Problem: Why is kube-proxy slow?
In traditional networking, packets travel through a complex maze of rules.

### 🔴 The "Traffic Hairpin" (Old Way)
Notice how the packet bounces around unnecessarily:

```mermaid
graph TD
    User[User Request] --> NIC[Network Interface]
    NIC --> Kernel
    Kernel -->|Slow Lookup| DNAT[DNAT Rules]
    DNAT --> PodA[Target Pod]
    
    style DNAT fill:#ff9999,stroke:#333,stroke-width:2px
