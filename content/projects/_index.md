---
title: Projects
description: Demos (MWC, Expo), research, and industrial projects
---


## Mobile World Congress (MWC) demos

I led and delivered multiple **AI-RAN** and **Open RAN** system demonstrations at **MWC Barcelona (2024–2026)**, with global industry partners, showcasing deployable **AI-driven 5G/6G** technologies.

**Selected demo playlists**

- **MWC 2026** — AI-RAN and O-RAN demos: [YouTube playlist](https://www.youtube.com/playlist?list=PLwzqEzG3j3NLQHQD25Yf9Yc2dK7w2MdwU)
- **MWC 2025** — AI-RAN and O-RAN demos: [YouTube playlist](https://www.youtube.com/playlist?list=PLwzqEzG3j3NJi5C01DX-baKSDvuY0oFKs)
- **MWC 2024** — O-RAN system demos: [YouTube playlist](https://www.youtube.com/playlist?list=PLwzqEzG3j3NK_E46yNXqIsH6OCUHGeetY)
- **MWC 2023** — O-RAN security demo: [watch on YouTube](https://www.youtube.com/watch?v=tOFZfmYThQw)

---

## World Expo 2025 (Singapore Pavilion, Osaka)

I coordinated **multi-company, cross-country** collaboration for an end-to-end **5G Non-Terrestrial Network (NTN)** live video-call demonstration over a **real GEO satellite link**, representing Singapore’s connectivity capabilities at **World Expo 2025**.

- **Demo video:** [YouTube — FCP / Expo NTN demo](https://www.youtube.com/watch?v=FCPXlupQKCc)

---

## Research projects

### 1. Collaboratively train anomaly detection models in edge computing (Research Fellow, SUTD)

- Research and implementation of a **federated learning** framework to collaboratively train an **anomaly detection (AD)** model for edge devices (e.g. multiple robots in different environments belonging to different cooperations).
- The AD model runs inference at the edge; we implemented **federated learning** so partners learn a **shared model** without leaking **private** training data.

### 2. Contextual-bandit anomaly detection for IoT data in hierarchical edge computing (Ph.D., SUTD)

- **Adaptive** selection among ML models using **contextual information** from input data for **anomaly detection**, formulated as a **contextual bandit** / single-step **Markov decision process**, optimizing **accuracy** and **delay**.
- Related publications (see also [Publications](/publications/)):
  - **ICDCS’20** — Mao V. Ngo, Tie Luo, Hakima Chaouchi, and Tony Q.S. Quek, “Contextual-Bandit Anomaly Detection for IoT Data in Distributed Hierarchical Edge Computing,” *IEEE ICDCS* — Demo Track, Jul. 2020. (Acceptance rate: 30%, 13/43.)
  - **AAAI’20 workshop** — Mao V. Ngo, Hakima Chaouchi, Tie Luo, Tony Q.S. Quek, “Adaptive Anomaly Detection for IoT Data in Hierarchical Edge Computing,” *AAAI Workshop on Artificial Intelligence of Things (AIoT)*, 2020.

### 3. Hierarchical mobile edge computing (HEC) for mobile users (Ph.D., SUTD)

- HEC architecture supporting **container migration** and **base station handover** for mobile users. **Source code:** [GitLab repository](https://gitlab.com/ngovanmao/edgecomputing).
- Real-world evaluation: **Android app** and offloaded services (e.g. face recognition with **OpenFace**, object detection with **YOLOv3**); sources released publicly.
- **Video demos:** [YouTube (1)](https://youtu.be/6FETIIdDqe8) · [YouTube (2)](https://youtu.be/7AzQ88y7K1M) · [Additional demo (OpenFace migration)](https://youtu.be/IqsHe43lHaw)
- **GLOBECOM’20** — Mao V. Ngo, Tie Luo, Hieu T. Hoang, and Tony Q.S. Quek, “Coordinated Container Migration and Base Station Handover in Mobile Edge Computing,” *IEEE GLOBECOM*, Dec. 2020.

### 4. Soldier Body Area Networks (Research Assistant, SUTD)

- Dynamic **resource management** for SBAN.
- **Interference** modeling and management for coexistence of single/multiple SBANs and other networks.
- **Energy-efficient MAC** for SBAN, with dynamic resource and interference management.
- **Context-aware scheduling** for energy efficiency and **emergency** traffic priority.
- **Heterogeneous** wireless integration for roaming and end-to-end **QoS**.
- Study of **operating band** effects on proposed techniques.

---

## Industrial projects

### 1. IPv6 for APZ system (R&D Engineer, DEK Vietnam)

- **Description:** APZ supports multiple IP stack implementations. An IP stack on the **Central Processor** handles internal I/O communication toward the **OSS**. This project implemented **IPv6** as a **layer-3** protocol on the **AXE** platform with **feature parity** to IPv4.
- **Skills:** TCP/IP, network programming, C/C++.

### 2. APZ central processor (CP) design (R&D Engineer, DEK Vietnam)

- Design **CP software blocks** in APZ for the [AXE telephone exchange](https://en.wikipedia.org/wiki/AXE_telephone_exchange) (BSC/TRC, MSC, HLR, SCP, FNR, TSC, STP, wireline nodes, etc.).
- Implementation in **[PLEX-C](https://en.wikipedia.org/wiki/PLEX_%28programming_language%29)** and ASA-C.
