# Assignment 3.2 - Part 2: Deep Dive Into the Selected Solution
## Quantum Computing Risks for Energy Sector Encryption

**Group 19**  
Ali Al Said - ama10643, Zixuan Shan - zs2985, Erica Belcena - eb4286, Anmol Vats - av3938

---

## 4. Threat Model

### 4.1 Threat Modeling Methodology

This section presents a comprehensive threat model for post-quantum cryptography implementation in energy sector infrastructure. We employ the STRIDE threat modeling methodology, originally developed by Microsoft engineers Loren Kohnfelder and Praerit Garg in 1999 [16], which has been successfully adapted for Industrial Control Systems (ICS) and energy sector applications [12, 13, 14]. STRIDE provides a systematic framework for identifying threats across six categories: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege [16, 18].

The STRIDE methodology is particularly suitable for cyber-physical systems in the energy sector because it enables contextual evaluation of solution- and sector-specific threats while encouraging early threat identification in the development cycle [18]. Recent studies have demonstrated STRIDE's effectiveness in analyzing power systems [12], distributed energy resources [14], and smart solar energy systems [15], making it an established approach for energy infrastructure security assessment.

Our threat modeling approach integrates STRIDE with post-quantum cryptography considerations to address both current and future security challenges. This hybrid methodology acknowledges that quantum computing poses unique threats to cryptographic systems [2, 3, 4, 6, 10], necessitating specialized analysis beyond traditional threat categories.

**Visual Artifacts:**
This threat model is accompanied by comprehensive Data Flow Diagrams (DFDs) and visual analysis artifacts:
- **Level 0 Context Diagram** (dfd_level0_context.png): High-level system architecture with trust boundaries
- **Level 1 Detailed DFD** (dfd_level1_detailed.png): Process decomposition with STRIDE threat annotations
- **Threat Severity Heatmap** (threat_heatmap.png): STRIDE categories vs. system components matrix
- **Attack Tree Diagram** (attack_tree.png): Quantum-enabled attack paths and progressions
- **Mitigation Timeline** (mitigation_timeline.png): PQC migration roadmap (2025-2036)

These diagrams are referenced throughout this analysis and are included in the accompanying PDF report.

### 4.2 System Architecture and Components

#### 4.2.1 Energy Sector System Decomposition

The modern energy sector comprises multiple interconnected components that rely on cryptographic protection:

**Operational Technology (OT) Layer:**
- Industrial Control Systems (ICS) and Supervisory Control and Data Acquisition (SCADA) systems [1, 12]
- Phasor Measurement Units (PMUs) for grid synchronization [1]
- Smart meters and IoT sensors [6, 7]
- Programmable Logic Controllers (PLCs) and Remote Terminal Units (RTUs) [13]

**Communication Infrastructure:**
- Virtual Private Networks (VPNs) for remote access [1]
- Wide Area Networks (WANs) connecting substations and control centers [12]
- Wireless sensor networks for distributed monitoring [2]
- Blockchain-based energy trading platforms [1, 5]

**Data Management Systems:**
- Energy Management Systems (EMS) [14]
- Distribution System Operators (DSO) platforms [14]
- Cloud-based data analytics and storage [6]
- Corporate networks and enterprise systems [3]

**Critical Assets at Risk:**
- Grid topology and power flow data [1]
- Infrastructure designs and vulnerability assessments [1]
- Operational commands and control signals [1]
- Authentication credentials and cryptographic keys [1, 4]
- Financial and trading information [5]
- Strategic planning documents [1]

**Reference Diagram:** See **dfd_level0_context.png** for a visual representation of the system context, showing external entities, trust boundaries, and quantum threat surface areas.

### 4.3 Trust Boundaries and Data Flows

Trust boundaries in energy systems separate different security domains and establish points where data transitions between trusted and untrusted zones [12, 14]. Key trust boundaries include:

1. **Corporate IT to Operational Technology (OT) boundary:** Separates business systems from control systems [13]
2. **Control Center to Field Devices boundary:** Marks the transition from centralized control to distributed equipment [12]
3. **Internal Network to Internet boundary:** Protects internal resources from external threats [14]
4. **Blockchain nodes to external users boundary:** Governs access to distributed energy trading systems [5]

Data flows across these boundaries include:
- Real-time sensor telemetry from field devices to control centers [1]
- Control commands from operators to ICS equipment [1, 13]
- Authentication tokens for remote access via VPN [1]
- PMU synchronization signals for grid frequency control [1]
- Blockchain transactions for energy trading [1, 5]
- Historical operational data for analytics and planning [3]

**Reference Diagram:** See **dfd_level1_detailed.png** for detailed process decomposition showing all data stores (D1-D5), processes (P1-P8), external entities, and annotated data flows with STRIDE threat identifiers. Trust zones are clearly marked with colored boundaries.

### 4.4 STRIDE-Based Quantum Threat Analysis

We now apply the STRIDE framework to identify specific threats emerging from quantum computing capabilities against energy sector cryptographic systems.

#### 4.4.1 Spoofing Threats

**Definition:** Spoofing involves impersonating another entity to gain unauthorized access or trust [16, 18].

**Quantum-Enabled Spoofing Threats:**

**S1: Authentication Token Forgery via Quantum Signature Breaking**
- **Threat Description:** Quantum computers using Shor's algorithm can break RSA and ECDSA digital signatures that authenticate VPN connections, allowing attackers to forge authentication tokens and impersonate legitimate users [1, 4, 9].
- **Attack Vector:** Adversaries capture VPN handshake data encrypted with RSA-2048 or ECC-based protocols (IKEv2, OpenVPN, IPSec), then use quantum computers to derive private keys and forge authentication credentials [1, 4].
- **Impact:** Unauthorized access to SCADA systems, substations, and control networks; potential for remote sabotage of generation and distribution systems [1, 4].
- **Affected Assets:** VPN infrastructure, Remote Terminal Units, field engineer credentials, control center access systems [1].
- **Timeline:** Critical risk window estimated between 2028-2036, when quantum computers achieve sufficient capability to break key exchange mechanisms [2, 3, 4, 10].
- **Likelihood:** High - VPN protocols widely deployed across energy infrastructure, captured traffic can be stored for future decryption [3, 4].

**S2: PMU Time Signal Spoofing**
- **Threat Description:** Quantum computers can break digital signatures protecting Phasor Measurement Unit (PMU) timestamp authentication, enabling attackers to forge synchronization signals [1].
- **Attack Vector:** Adversaries compromise ECC/RSA signatures used for PMU time and phasor authentication, injecting false synchronization data into the grid [1].
- **Impact:** Phase angle divergence, frequency instability, incorrect relay operations, cascading outages affecting regional grids [1].
- **Affected Assets:** PMU networks, time synchronization infrastructure, transmission line protection systems [1].
- **Timeline:** Immediate concern as PMU data has real-time operational impact [1].
- **Likelihood:** Medium-High - Requires sophisticated attack capabilities but has severe consequences [1].

**S3: Blockchain Identity Spoofing in Energy Trading**
- **Threat Description:** Quantum attacks on ECDSA signatures enable adversaries to derive private keys from public blockchain addresses, allowing impersonation of prosumers and energy traders [1, 5].
- **Attack Vector:** Attackers use quantum computers to solve elliptic curve discrete logarithm problem, obtaining private keys that control blockchain identities and smart contract execution [1, 5].
- **Impact:** Forged energy transactions, manipulated carbon credit ledgers, unauthorized smart contract executions causing billing errors [1, 5].
- **Affected Assets:** Distributed energy platforms, peer-to-peer trading systems, carbon accounting ledgers, smart contracts [1, 5].
- **Timeline:** Growing concern as blockchain adoption increases in energy sector [5].
- **Likelihood:** Medium - Blockchain transactions are publicly visible and permanently recorded, creating persistent attack surface [5].

#### 4.4.2 Tampering Threats

**Definition:** Tampering involves unauthorized modification of data or system components [16, 18].

**Quantum-Enabled Tampering Threats:**

**T1: ICS Command Manipulation via Encrypted Channel Compromise**
- **Threat Description:** Quantum computers break encryption protecting SCADA control channels, enabling adversaries to intercept and modify operational commands [1, 13].
- **Attack Vector:** Real-time control commands (e.g., adjusting compressor pressure, opening circuit breakers) transmitted over quantum-vulnerable encrypted channels are decrypted and modified in transit [1, 13].
- **Impact:** Equipment overloading, safety hazards, large-scale outages, physical damage to infrastructure, potential loss of life [1].
- **Affected Assets:** ICS/SCADA systems, field devices, operational setpoints, distribution control systems [1, 13].
- **Timeline:** Critical between 2030-2036 as quantum decryption becomes viable [2, 3, 10].
- **Likelihood:** High - Older embedded devices have minimal security and use vulnerable encryption [1].

**T2: Operational Data Modification via Quantum Decryption**
- **Threat Description:** Strategic plans, grid topology data, and operational analytics transmitted with classical encryption can be intercepted and modified by quantum-capable adversaries [1].
- **Attack Vector:** Attackers decrypt communications revealing infrastructure weaknesses, then inject modified data to manipulate decision-making or cover attack traces [1].
- **Impact:** Compromised grid reliability, inefficient resource allocation, incorrect contingency planning based on falsified data [1].
- **Affected Assets:** Energy management systems, grid topology databases, power flow models [1].
- **Timeline:** Risk increases as more historical data becomes decryptable [3, 11].
- **Likelihood:** Medium - Requires sustained access but provides strategic advantage [1].

**T3: Firmware and Software Update Tampering**
- **Threat Description:** Quantum attacks on code-signing certificates enable injection of malicious firmware updates into ICS devices and smart grid components [7, 9].
- **Attack Vector:** Adversaries break RSA/ECDSA signatures protecting firmware packages, replacing legitimate updates with compromised versions [7, 9].
- **Impact:** Persistent backdoors in critical infrastructure, long-term system compromise, potential for coordinated attacks [7].
- **Affected Assets:** ICS devices, smart meters, IoT sensors, network equipment [6, 7].
- **Timeline:** High-priority concern as firmware updates have long-term persistence [7].
- **Likelihood:** Medium-High - Software supply chain increasingly targeted by advanced threats [7].

#### 4.4.3 Repudiation Threats

**Definition:** Repudiation threats involve actors denying they performed specific actions, undermining accountability [16, 18].

**Quantum-Enabled Repudiation Threats:**

**R1: Non-Repudiation Failure in Energy Trading**
- **Threat Description:** Quantum-broken digital signatures allow traders to deny executing energy transactions or smart contract operations after the fact [5].
- **Attack Vector:** Attackers break ECDSA signatures on blockchain transactions retroactively, claiming transactions were forged [5].
- **Impact:** Market instability, loss of trust in decentralized energy platforms, regulatory compliance failures [5].
- **Affected Assets:** Energy trading records, smart contract logs, billing systems [5].
- **Timeline:** Future threat as quantum capabilities mature [5].
- **Likelihood:** Medium - Depends on transaction dispute mechanisms and legal frameworks [5].

**R2: Audit Trail Compromise in Critical Operations**
- **Threat Description:** Quantum decryption of signed audit logs allows adversaries to claim operational records were tampered with, undermining forensic investigations [12, 13].
- **Attack Vector:** Breaking cryptographic signatures on SCADA logs and operational records [13].
- **Impact:** Inability to establish accountability after security incidents, compromised legal proceedings [13].
- **Affected Assets:** SIEM systems, audit logs, incident response records [12, 13].
- **Timeline:** Growing concern as digital forensics become primary evidence source [13].
- **Likelihood:** Low-Medium - Requires sophisticated attack and legal exploitation [13].

#### 4.4.4 Information Disclosure Threats

**Definition:** Information disclosure involves exposing confidential data to unauthorized parties [16, 18].

**Quantum-Enabled Information Disclosure Threats:**

**I1: Harvest Now, Decrypt Later (HNDL) Attacks on Strategic Energy Data**
- **Threat Description:** Nation-state adversaries intercept and store encrypted energy sector communications today for future quantum decryption, targeting data with 20-50 year operational relevance [1, 3, 8, 11].
- **Attack Vector:** Passive collection of encrypted traffic containing infrastructure designs, vulnerability assessments, merger negotiations, cybersecurity incident reports, and long-term energy policies [1, 3].
- **Impact:** Exposure of infrastructure weaknesses exploitable decades later, loss of competitive advantage, national security compromise, retroactive espionage [1, 3].
- **Affected Assets:** Strategic planning documents, infrastructure blueprints, M&A communications, vulnerability reports [1, 3].
- **Timeline:** Active threat NOW - data being harvested while still encrypted [3, 8, 11].
- **Likelihood:** Very High - Known adversary activity, low barrier to passive collection [3, 8, 11].
- **Urgency:** CRITICAL - Requires immediate PQC implementation for sensitive long-term data [3, 8].

**I2: Real-Time Operational Data Exposure**
- **Threat Description:** Quantum breaking of communication channel encryption exposes real-time grid operational data, power flows, and system vulnerabilities [1, 2].
- **Attack Vector:** Decryption of SCADA telemetry, EMS data streams, and control center communications [1, 2].
- **Impact:** Operational visibility provides attackers roadmap for cyber-physical attacks, market manipulation opportunities, energy price disruption [1, 2].
- **Affected Assets:** Grid topology data, power flow models, real-time telemetry, load forecasting data [1, 2].
- **Timeline:** 2030-2036 quantum capability window [2, 3, 10].
- **Likelihood:** High - Real-time data continuously transmitted over potentially vulnerable channels [1, 2].

**I3: Credentials and Key Material Exposure**
- **Threat Description:** Quantum computers can extract symmetric encryption keys from captured key exchange protocols, exposing all session data [4, 9].
- **Attack Vector:** Breaking Diffie-Hellman or RSA key exchanges to recover AES/3DES session keys used in VPN tunnels [1, 4, 9].
- **Impact:** Mass credential compromise, persistent network access, cascading breaches across interconnected systems [1, 4].
- **Affected Assets:** VPN credentials, certificate private keys, API tokens, database passwords [1, 4].
- **Timeline:** High priority as credentials enable further attacks [4].
- **Likelihood:** Very High - Key exchange protocols universally vulnerable to Shor's algorithm [4, 9].

**I4: Smart Grid Privacy Violations**
- **Threat Description:** Quantum decryption of smart meter data exposes consumer energy usage patterns, enabling privacy violations and targeted attacks [6, 7].
- **Attack Vector:** Breaking AES encryption on smart meter communications (vulnerable to Grover's algorithm, though less severe than asymmetric threats) [2, 6].
- **Impact:** Privacy breaches, consumer profiling, burglary targeting (identifying when homes are unoccupied) [6, 7].
- **Affected Assets:** Smart meter data, Advanced Metering Infrastructure (AMI), customer databases [6, 7].
- **Timeline:** Secondary concern; symmetric encryption more resistant than asymmetric [2, 9].
- **Likelihood:** Medium - Requires targeting individual consumers, lower value than infrastructure attacks [6].

#### 4.4.5 Denial of Service Threats

**Definition:** Denial of Service (DoS) threats aim to disrupt system availability and prevent legitimate users from accessing resources [16, 18].

**Quantum-Enabled Denial of Service Threats:**

**D1: Cryptographic Protocol Exhaustion**
- **Threat Description:** Implementing post-quantum cryptographic algorithms with larger key sizes and computational overhead may create performance bottlenecks in resource-constrained ICS devices [6, 7, 9].
- **Attack Vector:** Adversaries exploit increased computational demands of PQC algorithms (e.g., lattice-based schemes) to overwhelm low-power field devices with cryptographic operations [6, 9].
- **Impact:** Communication delays, control loop instability, device crashes, operational disruptions [6, 7, 9].
- **Affected Assets:** Embedded ICS controllers, smart meters, IoT sensors, legacy SCADA devices [6, 7, 9].
- **Timeline:** Immediate concern during PQC migration phase [6, 7].
- **Likelihood:** Medium-High - Resource constraints well-documented challenge for PQC deployment [6, 9].

**D2: Quantum-Enhanced Distributed Denial of Service (DDoS)**
- **Threat Description:** While not directly quantum-related, compromised systems (via quantum-broken authentication) can be leveraged for amplified DDoS attacks against energy infrastructure [12, 13].
- **Attack Vector:** Attackers use quantum-compromised credentials to control large numbers of IoT devices and launch coordinated DDoS attacks [6, 12].
- **Impact:** Energy management system outages, control center communication failures, loss of real-time monitoring capabilities [12].
- **Affected Assets:** Network infrastructure, control center servers, communication gateways [12].
- **Timeline:** Secondary effect following initial quantum-enabled compromise [12].
- **Likelihood:** Medium - Requires combination of quantum breakthrough and botnet capabilities [6, 12].

**D3: Time Synchronization Denial via GPS Spoofing**
- **Threat Description:** Quantum-broken authentication of GPS signals (if cryptographically protected) or timing protocols enables denial of accurate time synchronization [1].
- **Attack Vector:** Spoofing GPS/GNSS signals or breaking authentication on network time protocols [1].
- **Impact:** Grid desynchronization, PMU data corruption, inability to correlate events across wide-area networks [1].
- **Affected Assets:** Time synchronization infrastructure, GPS receivers, PMU networks [1].
- **Timeline:** Critical during grid operation [1].
- **Likelihood:** Medium - Requires sophisticated GPS spoofing capabilities [1].

#### 4.4.6 Elevation of Privilege Threats

**Definition:** Elevation of privilege involves gaining unauthorized access to higher-level system privileges or administrative control [16, 18].

**Quantum-Enabled Elevation of Privilege Threats:**

**E1: Certificate Authority Compromise**
- **Threat Description:** Quantum computers can break root certificate authority (CA) private keys, enabling adversaries to issue fraudulent certificates with full trust chain validation [4, 10].
- **Attack Vector:** Breaking RSA/ECDSA private keys of energy sector CAs or upstream public CAs, then issuing certificates for arbitrary domains/entities [4, 10].
- **Impact:** Complete trust infrastructure collapse, ability to impersonate any entity, man-in-the-middle attacks on all encrypted connections [4, 10].
- **Affected Assets:** PKI infrastructure, TLS/SSL certificates, code-signing certificates, device identity certificates [4, 10].
- **Timeline:** CRITICAL - Single CA compromise affects entire trust ecosystem [4, 10].
- **Likelihood:** High - CAs are high-value targets with centralized impact [4, 10].

**E2: Privilege Escalation via Compromised Administrative Credentials**
- **Threat Description:** Quantum-broken VPN authentication enables lateral movement from field engineer access to administrative control systems [1, 4, 13].
- **Attack Vector:** Using quantum-compromised credentials to progressively access higher privilege levels within segmented networks [4, 13].
- **Impact:** Full control over operational technology, ability to modify protection settings, disable security controls [13].
- **Affected Assets:** Domain controllers, administrative interfaces, privilege access management systems [13].
- **Timeline:** High priority as administrative access enables persistent control [13].
- **Likelihood:** High - Privilege escalation common attack pattern [13].

**E3: Smart Contract Privilege Exploitation**
- **Threat Description:** Quantum attacks on blockchain signatures enable adversaries to execute privileged smart contract functions reserved for specific roles (e.g., grid operators, market administrators) [5].
- **Attack Vector:** Deriving private keys for privileged blockchain accounts, then calling administrative smart contract functions [5].
- **Impact:** Market manipulation, unauthorized fund transfers, modification of trading rules, disruption of decentralized energy markets [5].
- **Affected Assets:** Smart contracts, governance tokens, privileged blockchain accounts [5].
- **Timeline:** Growing concern as DeFi energy platforms mature [5].
- **Likelihood:** Medium - Requires quantum capability plus blockchain expertise [5].

### 4.5 Threat Prioritization Matrix

Based on the STRIDE analysis, we prioritize threats using impact, likelihood, and timeline considerations:

| Priority | Threat ID | Category | Threat Name | Impact | Likelihood | Timeline |
|----------|-----------|----------|-------------|---------|------------|----------|
| **CRITICAL** | I1 | Information Disclosure | HNDL Attacks | Very High | Very High | NOW |
| **CRITICAL** | E1 | Elevation of Privilege | CA Compromise | Very High | High | 2028-2036 |
| **HIGH** | S1 | Spoofing | VPN Auth Forgery | High | High | 2028-2036 |
| **HIGH** | T1 | Tampering | ICS Command Manipulation | Very High | High | 2030-2036 |
| **HIGH** | I3 | Information Disclosure | Credential Exposure | High | Very High | 2028-2036 |
| **HIGH** | E2 | Elevation of Privilege | Admin Privilege Escalation | High | High | 2028-2036 |
| **MEDIUM-HIGH** | S2 | Spoofing | PMU Signal Spoofing | Very High | Medium | NOW |
| **MEDIUM-HIGH** | T3 | Tampering | Firmware Tampering | High | Medium-High | 2028-2036 |
| **MEDIUM-HIGH** | D1 | Denial of Service | PQC Performance DoS | Medium | Medium-High | NOW |
| **MEDIUM** | S3 | Spoofing | Blockchain Identity Spoofing | High | Medium | 2030-2036 |
| **MEDIUM** | T2 | Tampering | Data Modification | Medium | Medium | 2030-2036 |
| **MEDIUM** | I2 | Information Disclosure | Real-time Data Exposure | High | High | 2030-2036 |
| **MEDIUM** | D2 | Denial of Service | Quantum-enabled DDoS | Medium | Medium | 2030-2036 |
| **MEDIUM** | E3 | Elevation of Privilege | Smart Contract Exploit | High | Medium | 2030-2036 |
| **LOW-MEDIUM** | R1 | Repudiation | Trading Non-Repudiation | Medium | Medium | 2030-2036 |
| **LOW-MEDIUM** | R2 | Repudiation | Audit Log Repudiation | Low | Low-Medium | 2030-2036 |
| **LOW-MEDIUM** | I4 | Information Disclosure | Smart Meter Privacy | Medium | Medium | 2032+ |
| **LOW-MEDIUM** | D3 | Denial of Service | Time Sync Denial | High | Medium | NOW |

**Reference Diagram:** See **threat_heatmap.png** for a visual matrix representation showing threat severity (1=Low to 4=Critical) across all STRIDE categories for each system component. This heatmap provides an at-a-glance view of where the most critical vulnerabilities exist.

### 4.6 Attack Paths and Kill Chain Analysis

Following established cyber kill chain methodology [14], we map quantum-enabled attack progressions:

**Attack Path 1: HNDL → Strategic Infrastructure Compromise**
1. **Reconnaissance:** Identify high-value communication channels (control center to substations, corporate strategic planning)
2. **Weaponization:** Deploy passive collection infrastructure to capture encrypted traffic
3. **Delivery:** Not applicable (passive collection)
4. **Exploitation:** Wait for quantum computing capability, then decrypt archived data using Shor's algorithm
5. **Installation:** Use discovered vulnerabilities from decrypted assessments to establish persistent access
6. **Command & Control:** Leverage infrastructure knowledge to navigate networks
7. **Actions on Objectives:** Sabotage operations, exfiltrate additional data, or prepare for cyber-physical attacks

**Attack Path 2: VPN Compromise → Lateral Movement → ICS Control**
1. **Reconnaissance:** Identify VPN endpoints used by field engineers and remote operators
2. **Weaponization:** Capture VPN handshakes encrypted with RSA/ECDSA
3. **Delivery:** Quantum computer breaks key exchange
4. **Exploitation:** Use recovered credentials to authenticate to VPN
5. **Installation:** Deploy tools for privilege escalation and lateral movement
6. **Command & Control:** Establish backdoor in operational network
7. **Actions on Objectives:** Send malicious commands to ICS devices, manipulate operational setpoints

**Attack Path 3: CA Compromise → Trust Infrastructure Collapse**
1. **Reconnaissance:** Identify certificate authorities used by energy sector organizations
2. **Weaponization:** Obtain CA public certificates, apply quantum algorithm to derive private keys
3. **Delivery:** Not applicable (cryptographic attack)
4. **Exploitation:** Issue fraudulent certificates for critical infrastructure systems
5. **Installation:** Deploy man-in-the-middle infrastructure using valid certificates
6. **Command & Control:** Intercept and modify all encrypted communications
7. **Actions on Objectives:** Large-scale data exfiltration, command injection, widespread disruption

**Reference Diagram:** See **attack_tree.png** for a comprehensive attack tree visualization showing how quantum computing capabilities (Shor's and Grover's algorithms) enable progression through attack steps to ultimate compromise. The diagram shows AND/OR logic gates indicating which attack paths must be completed together versus alternatives.

### 4.7 Threat Actors and Motivations

**Nation-State Actors:**
- **Capabilities:** Advanced persistent threats (APTs) with quantum computing research programs and substantial collection infrastructure [3, 8, 11]
- **Motivations:** Strategic intelligence, infrastructure weakening for geopolitical leverage, preparation for future conflicts [3, 8]
- **Target Threats:** I1 (HNDL), E1 (CA Compromise), I2 (Operational Data), T1 (ICS Manipulation)
- **Timeline:** Already active in data collection; quantum exploitation 2030-2036 [3, 8, 10, 11]

**Cybercriminal Organizations:**
- **Capabilities:** Sophisticated ransomware operations, access to cryptocurrency resources, potentially purchasing quantum computing access [10]
- **Motivations:** Financial gain through extortion, data theft, cryptocurrency mining [10]
- **Target Threats:** S1 (VPN Compromise), T3 (Firmware Tampering), E2 (Privilege Escalation)
- **Timeline:** Active exploitation following quantum availability [10]

**Insider Threats:**
- **Capabilities:** Legitimate access credentials, knowledge of internal systems, potential collaboration with external actors [13]
- **Motivations:** Financial gain, revenge, ideological reasons, coercion [13]
- **Target Threats:** R1 (Non-repudiation), I2 (Data Exposure), T2 (Data Modification)
- **Timeline:** Continuous risk [13]

**Hacktivist Groups:**
- **Capabilities:** Variable technical expertise, potential access to quantum computing via cloud services [10]
- **Motivations:** Political or environmental activism, public attention, disruption of energy operations [10]
- **Target Threats:** D2 (DDoS), S2 (PMU Spoofing), T1 (Command Manipulation)
- **Timeline:** Opportunistic exploitation as tools become available [10]

### 4.8 Regulatory and Compliance Context

Several regulatory frameworks drive the urgency of quantum-resistant security for critical infrastructure:

**European Union:**
- **NIS2 Directive (Network and Information Security 2):** Requires strengthened cyber resilience for energy sector operators, with explicit consideration of emerging threats including quantum computing [4, 8, 10]
- **Digital Operational Resilience Act (DORA):** Mandates operational robustness against technological threats, applicable to energy sector financial operations [4, 10]
- **Timeline:** Compliance requirements increasingly incorporate PQC by 2028-2031 [8, 10]

**United States:**
- **Presidential Executive Order (May 2022):** Directs federal agencies to transition to quantum-resistant algorithms by 2035 [11]
- **NIST Post-Quantum Cryptography Standards:** Finalized in 2024, providing approved algorithms (CRYSTALS-Kyber, CRYSTALS-Dilithium, SPHINCS+) for government and critical infrastructure [4, 6, 9]
- **CISA Guidance:** Recommends immediate crypto-agility planning and PQC migration roadmaps [1]

**United Kingdom:**
- **NCSC PQC Roadmap:** Three-phase transition plan requiring critical infrastructure (energy, banking, telecom) to complete migration by mid-2030s [8, 10]
- **Phases:** (1) 2023-2028: Cryptographic inventory and planning; (2) 2028-2031: Critical system upgrades; (3) 2031-2035: Full migration [8]

**Germany:**
- **BSI (Federal Office for Information Security):** Certified world's first quantum-safe smartcard (January 2025); deployed PQC in military fiber optic networks [8]
- **Energy Sector Requirements:** Integration of PQC in critical energy data infrastructure projects [8]

### 4.9 Assumptions and Constraints

**Assumptions:**
1. Cryptographically Relevant Quantum Computer (CRQC) capable of breaking RSA-2048 and ECC-256 will be available between 2030-2036 [2, 3, 4, 10]
2. Adversaries are currently conducting Harvest Now, Decrypt Later (HNDL) attacks on valuable encrypted traffic [3, 8, 11]
3. Energy infrastructure has 20-50 year operational lifespans, meaning data encrypted today must remain secure for decades [1, 3]
4. NIST-standardized PQC algorithms (Kyber, Dilithium, Falcon, SPHINCS+) provide adequate post-quantum security [4, 6, 9]
5. Symmetric encryption (AES-256) remains secure against quantum attacks (Grover's algorithm provides only quadratic speedup) [2, 9]

**Constraints:**
1. **Resource Limitations:** Many ICS devices have limited computational power, memory, and energy, constraining PQC algorithm deployment [6, 7, 9]
2. **Legacy Infrastructure:** Older SCADA and control systems cannot be easily upgraded and may require hardware replacement [1, 13]
3. **Operational Continuity:** Energy systems operate 24/7 with high availability requirements, limiting maintenance windows for cryptographic migration [12, 13]
4. **Interoperability:** Energy networks involve multiple vendors, protocols, and standards, complicating coordinated PQC adoption [2, 6]
5. **Cost:** Full cryptographic infrastructure replacement represents significant capital expenditure [7, 10]
6. **Timeline Pressure:** Migration must complete before CRQC availability, creating compressed implementation schedule [3, 8, 10]

### 4.10 Mitigation Strategy Framework

Based on the STRIDE threat analysis, we propose a layered mitigation framework addressing quantum threats:

**Layer 1: Immediate Actions (2025-2028)**
- Implement hybrid classical/PQC encryption for high-value communications (addresses I1: HNDL) [3, 4]
- Conduct cryptographic inventory across all energy sector systems [8, 10]
- Deploy crypto-agile infrastructure enabling rapid algorithm updates [10, 11]
- Re-encrypt strategic data archives using quantum-resistant algorithms [3]
- Implement enhanced monitoring for VPN anomalies and authentication failures [1, 4]

**Layer 2: Medium-Term Transition (2028-2032)**
- Migrate VPN infrastructure to post-quantum key exchange (Kyber) and signatures (Dilithium/Falcon) [1, 4, 6]
- Upgrade PMU authentication to PQC-based digital signatures [1]
- Implement quantum-safe blockchain signature schemes for energy trading platforms [5]
- Replace ICS device certificates with PQC-based PKI [1, 6]
- Deploy hardware security modules (HSMs) with quantum-resistant capabilities [4]

**Layer 3: Long-Term Hardening (2032-2036)**
- Complete migration of all cryptographic systems to PQC standards [8, 10]
- Implement multi-source time synchronization with quantum-safe authentication [1]
- Deploy physics-based anomaly detection for impossible operational states (complements cryptographic controls) [1]
- Establish trusted execution environments (TEEs) for critical smart contract validation [1, 5]
- Implement quantum key distribution (QKD) for highest-security connections (where physically feasible) [2]

**Layer 4: Defense-in-Depth Enhancements**
- Network segmentation and zero-trust access controls (reduces lateral movement) [1, 13]
- Hardware-based multi-factor authentication not relying solely on cryptographic key exchange [1, 4]
- Physical safety interlocks preventing impossible operational states regardless of software commands [1]
- Regular quantum readiness assessments and penetration testing [10]
- Incident response plans specifically addressing quantum compromise scenarios [11]

**Reference Diagram:** See **mitigation_timeline.png** for a Gantt-chart style visualization of all mitigation activities from 2025-2036, color-coded by priority layer and showing critical quantum threat milestones. This timeline provides a clear roadmap for PQC migration planning and execution.

### 4.11 Validation Against Real-World Incidents

To validate our threat model, we examine parallels with historical cyber incidents targeting energy infrastructure:

**Stuxnet (2010) - Iran Nuclear Facilities:**
- **Relevance:** Demonstrated feasibility of manipulating ICS operational parameters (T1: ICS Command Manipulation) [13]
- **Quantum Dimension:** Future quantum-enabled attacks could break authentication protecting control channels, enabling similar manipulation without physical access [1, 13]

**BlackEnergy 3 (2015) - Ukraine Power Grid:**
- **Relevance:** Showed vulnerability of energy SCADA systems to remote compromise (S1: VPN Authentication Forgery, T1) [13]
- **Quantum Dimension:** Quantum-broken VPN credentials would provide similar remote access without exploiting software vulnerabilities [1, 13]

**Triton/Trisis (2017) - Saudi Arabian Chemical Plant:**
- **Relevance:** Targeted industrial safety systems, demonstrating potential for physical harm (T1: ICS Tampering) [13]
- **Quantum Dimension:** Quantum compromise of safety system authentication could bypass protections designed to prevent exactly these attacks [1, 13]

**Colonial Pipeline Ransomware (2021):**
- **Relevance:** Demonstrated cascading impact of credential compromise on critical energy infrastructure [13]
- **Quantum Dimension:** Quantum-enabled credential theft (I3) would provide similar access for ransomware deployment or operational disruption [1, 4]

**SolarWinds Supply Chain Attack (2020):**
- **Relevance:** Showed impact of compromised software updates (T3: Firmware Tampering) [13]
- **Quantum Dimension:** Quantum-broken code-signing certificates would enable similar supply chain attacks without initial software compromise [7]

These historical incidents validate that the threats identified in our STRIDE model reflect real attack patterns. The quantum dimension amplifies these threats by eliminating the need for software vulnerabilities or insider access—purely cryptographic attacks become sufficient.

---

## References

[1] CISA. (2024). "Post-Quantum Considerations for Operational Technology." Cybersecurity and Infrastructure Security Agency. https://www.cisa.gov/sites/default/files/2024-10/Post-Quantum%20Considerations%20for%20Operational%20Technology%20%28508%29.pdf

[2] Giani, A., & Eldredge, Z. (2021). "Quantum computing opportunities in renewable energy." SN Computer Science, 2(5), 393. https://link.springer.com/article/10.1007/s42979-021-00786-3

[3] Microsoft Security. (2023). "Quantum Computing: Harvest Now, Decrypt Later." Microsoft Security Blog. https://www.microsoft.com/en-us/security/blog/2023/06/01/quantum-computing-harvest-now-decrypt-later/

[4] Baseri, Y., et al. (2024). "Cybersecurity in the Quantum Era: Assessing the Impact of Quantum Computing on Infrastructure." arXiv preprint arXiv:2404.10659. https://arxiv.org/abs/2404.10659

[5] Kumar, P., et al. (2025). "Decentralized Energy Trading in Smart Grid Using Secured Post Quantum Encryption." ScienceDirect. https://www.sciencedirect.com/science/article/pii/S2590123025018389

[6] Sajimon, P. C., et al. (2024). "POST-QUANTUM CRYPTOGRAPHY FOR SECURING FUTURE-PROOF SMART ENERGY INFRASTRUCTURE." ResearchGate. https://www.researchgate.net/publication/392734298_POST-QUANTUM_CRYPTOGRAPHY_FOR_SECURING_FUTURE-PROOF_SMART_ENERGY_INFRASTRUCTURE

[7] Nguyen, T., et al. (2025). "Enhancing IoT security in smart grids with quantum-resistant hybrid encryption." Scientific Reports. https://www.nature.com/articles/s41598-024-84427-8

[8] Spherity. (2025). "Countdown to Quantum Threat: Upgrade Your Digital Identity With A PQC-ready European Business Wallet." Medium. https://medium.com/spherity/countdown-to-quantum-threat-upgrade-your-digital-identity-with-a-pqc-ready-european-business-c64f16dbde52

[9] Zhang, Y., et al. (2025). "Evaluating Post-Quantum Cryptographic Algorithms on Resource-Constrained Devices." arXiv preprint arXiv:2507.08312v1. https://arxiv.org/html/2507.08312v1

[10] BCG. (2025). "How Quantum Computing Will Upend Cybersecurity." Boston Consulting Group. https://www.bcg.com/publications/2025/how-quantum-computing-will-upend-cybersecurity

[11] PwC. (2024). "The Quantum Conundrum: How to prepare now." PwC Global. https://www.pwc.com/gx/en/issues/cybersecurity/the-quantum-conundrum.html

[12] Al-Hamadi, H., et al. (2025). "Threat Analysis of Power System Case Study via STRIDE Threat Model in Digital Twin Real-time Platform." ResearchGate. https://www.researchgate.net/publication/393201659_Threat_Analysis_of_Power_System_Case_Study_via_STRIDE_Threat_Model_in_Digital_Twin_Real-time_Platform

[13] Kim, J., et al. (2022). "STRIDE-based threat modeling and DREAD evaluation for the distributed control system in the oil refinery." ETRI Journal, Wiley Online Library. https://onlinelibrary.wiley.com/doi/full/10.4218/etrij.2021-0181

[14] Khan, M. A., et al. (2024). "A Comprehensive Threat Modelling Analysis for Distributed Energy Resources." ACM Transactions on Cyber-Physical Systems. https://dl.acm.org/doi/10.1145/3678260

[15] ResearchGate. (2024). "STRIDE-based Methodologies for Threat Modeling of Industrial Control Systems: A Review." https://www.researchgate.net/publication/383452204_STRIDE-based_Methodologies_for_Threat_Modeling_of_Industrial_Control_Systems_A_Review

[16] Wikipedia. (2025). "STRIDE model." https://en.wikipedia.org/wiki/STRIDE_model

[17] Khan, R., et al. (2022). "Threat Modeling of Cyber-Physical Systems - A Case Study of a Microgrid System." ScienceDirect. https://www.sciencedirect.com/science/article/pii/S016740482200342X

[18] IriusRisk. (2025). "Threat modeling STRIDE methodology." https://www.iriusrisk.com/resources-blog/threat-modeling-methodology-stride

[19] RAND Corporation. (2023). "When a quantum computer is able to break our encryption." https://www.rand.org/pubs/commentary/2023/09/when-a-quantum-computer-is-able-to-break-our-encryption.html

[20] Cambridge Consultants. (2025). "Energy network cybersecurity and post quantum cryptography." https://www.cambridgeconsultants.com/quantum-tools-to-tackle-the-cybersecurity-threat-to-energy/

[21] Alvarez & Marsal. (2025). "Post-Quantum Cryptography: Strategic Implications for Key Sectors in Europe." https://www.alvarezandmarsal.com/thought-leadership/post-quantum-cryptography-strategic-implications-for-key-sectors-in-europe

[22] Lee, J., et al. (2022). "Toward Quantum Secured Distributed Energy Resources: Adoption of Post-Quantum Cryptography (PQC) and Quantum Key Distribution (QKD)." MDPI Energies. https://www.mdpi.com/1996-1073/15/3/714
