# 12th Project - Cybersecurity Risk Management Report
**Arab Academy for Science, Technology and Maritime Transport**  
**Course:** Risk Management (Project Part 2)

---

## Team Members
*   **Nada Ibrahim** | 221003011
*   **Mawada Ayman** | 221003907
*   **Haidy Hassanin** | 221003000
*   **Habiba Hesham** | 221003746

*   **Lecturer:** Dr. Nada Hany
*   **TA:** Eng. Nour Ehab

---

## Task 1: Risk Identification and Statements

### 1. Identified Cybersecurity Risks
Below is the extraction of 5 distinct risks distributed across network, application, operations, and physical domains based on the GHU infrastructure study:

*   **R1 (Network):** The Labs VLAN is configured to route traffic directly to the Servers VLAN without passing through a firewall.
*   **R2 (Application):** The payment gateway API key is hardcoded directly into the frontend JavaScript code.
*   **R3 (Operations):** The backup NAS storage device is located on the same network segment and Active Directory domain as the production environment.
*   **R4 (Network/IoT):** Biometric IoT attendance devices are running on default administrator credentials and placed on the same VLAN as the Student WiFi.
*   **R5 (Physical):** Server closet doors located inside the student computer labs are frequently left open and physically unsecured.

### 2. Formal Risk Statements (Cause-Event-Impact Format)
*   **R1:** Due to the misconfiguration allowing the Labs VLAN to communicate directly with the Servers VLAN without firewall inspection, attackers may gain unauthorized access to critical university servers, leading to sensitive data exposure and service disruption.
*   **R2:** Due to the payment gateway API key being hardcoded in the frontend JavaScript and publicly visible, attackers may steal and misuse the API credentials, resulting in unauthorized financial transactions and potential data compromise.
*   **R3:** Due to the backup NAS sharing the same network segment and Active Directory domain as the production environment, ransomware attacks may spread to the backup systems, leading to backup deletion and permanent data loss.
*   **R4:** Due to many biometric IoT devices still using default administrator credentials and being connected to the same VLAN as the student WiFi network, attackers may compromise these devices and gain unauthorized network access, resulting in manipulation of attendance records and exposure of biometric data.
*   **R5:** Due to server closet doors inside computer labs frequently being left open, unauthorized individuals may gain physical access to network infrastructure and servers, leading to device tampering or service interruption.

---

## Task 2: Vulnerability Assessment (CVSS v4.0)

**Target Vulnerability:** Hardcoded Payment Gateway API Key in Frontend JavaScript (R2)

### Metric Justification
*   **Attack Vector (AV): Network (N)** -> The key is embedded in the frontend code, making it accessible to anyone over the internet.
*   **Attack Complexity (AC): Low (L)** -> Viewing the source code requires no technical complexity or specialized conditions.
*   **Attack Requirements (AT): None (N)** -> No specific architecture or deployment configurations are needed for this to be exploited.
*   **Privileges Required (PR): None (N)** -> The attacker does not need an active session or account to see the frontend code.
*   **User Interaction (UI): None (N)** -> Extraction of the key can be done independently without tricking a legitimate user.
*   **Vulnerable System Confidentiality (VC): High (H)** -> Total exposure of a critical financial API credential.
*   **Vulnerable System Integrity (VI): None (N)** -> The GHU web server code itself isn't modified by just viewing the key.
*   **Vulnerable System Availability (VA): None (N)** -> The visibility of the key does not cause immediate downtime to the portal.
*   **Subsequent System Confidentiality (SC): High (H)** -> The stolen key grants unauthorized visibility into the third-party payment gateway's transaction logs and data.
*   **Subsequent System Integrity (SI): High (H)** -> Attackers can use the key to execute fraudulent payment operations directly on the gateway.
*   **Subsequent System Availability (SA): None (N)** -> There is no explicit indication that the payment provider's services will suffer an outage.

### Base Score & Vector
*   **Vector String:** `CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:H/SI:H/SA:N`
*   **CVSS v4.0 Score:** **9.3 (Critical)**

---

## Task 3: Quantitative Risk Assessment (FAIR)

**Scenario:** Loss exposure resulting from attackers scraping the hardcoded payment API key in GHU's frontend JavaScript to execute unauthorized transactions within a 12-month timeframe.

### 1. Loss Event Frequency (LEF) Calculation
*   **Contact Frequency (CF):** Estimated at 5,000 contacts/year (representing users/scanners inspecting the source code out of 20,000 total students).
*   **Probability of Action (PoA):** Estimated at 2% (0.02) due to high financial incentives, lack of rate limiting, and disabled WAF during peak registration.
*   **Threat Event Frequency (TEF):** 5,000 * 0.02 = 100 threat events/year.
*   **Susceptibility (Susc):** Set at 30% (0.30) because Threat Capability (TCap) vastly exceeds GHU's Resistance Strength (RS) given the lack of monitoring/SOC.
*   **Final LEF:** 100 * 0.30 = **30 loss events/year**.

### 2. Loss Magnitude (LM) Breakdown
*   **Primary Loss (PL):** Range between \$9,000 and \$26,000 per incident (Direct fraudulent transaction costs + IT response + key replacement expenses).
*   **Secondary Loss (SL):** Driven by an 80% Secondary Loss Event Frequency (SLEF = 0.80).
    *   *Regulatory Fines (PCI-DSS):* Most likely \$60,000
    *   *Reputation Damage:* Most likely \$31,500
    *   *Legal Liability:* Most likely \$20,000
    *   *Most Likely Secondary Loss Magnitude (SLM):* \$111,500
*   **Expected Secondary Loss (SL):** 0.80 * \$111,500 = \$89,200.
*   **Total Most Likely LM:** \$17,500 (Average Primary) + \$89,200 = **\$106,700 per event**.

### 3. Annual Loss Exposure (ALE)
*   **Formula:** ALE = LEF * Most Likely LM
*   **Calculation:** 30 * \$106,700 = **\$3,201,000 / year**

---

## Task 4: Qualitative Risk Assessment

### 1. Risk Register

| Risk ID | Affected Asset | Existing Controls | Likelihood | Impact | Score | Level | Mitigation Strategy |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **R1** | University Servers / DBs | Basic, misconfigured VLAN routing | 5 | 5 | **25** | Critical | Reconfigure VLAN boundaries and enforce strict firewall inspection. |
| **R2** | Payment Gateway System | HTTPS encryption only (No API protection) | 5 | 5 | **25** | Critical | Migrate API keys to backend environments; establish key rotation. |
| **R3** | Backup Infrastructure | Active, non-isolated daily network backups | 4 | 5 | **20** | High | Segregate backup segments; deploy offline/air-gapped storage. |
| **R4** | Biometric / Attendance Sys | Basic network placement, default logins | 5 | 5 | **25** | Critical | Enforce credential changes; isolate IoT devices on a separate VLAN. |
| **R5** | Switches & Server Closets | General lab supervision only | 3 | 4 | **12** | High | Install physical locks and implement strict access control policies. |

### 2. Risk Matrix Heat Map (5x5)

*   **5 (Almost Certain):** R1, R2, R4 *(at Impact 5)*
*   **4 (Likely):** R3 *(at Impact 5)*
*   **3 (Possible):** R5 *(at Impact 4)*
*   **2 (Unlikely):** None
*   **1 (Rare):** None

### 3. Bow-Tie Analysis Blueprint (For R2)
*   **Root Causes:**
    1. Developers hardcoding credentials directly into frontend JavaScript scripts.
    2. Disabling the WAF environment during heavy registration weeks to mitigate load.
*   **Core Event:** Compromised Payment API Credentials.
*   **Preventative Controls:**
    1. Implementing Server-Side Secret Management architectures.
    2. Deploying automated static code analysis / secret scanning tools in CI/CD.
    3. Keeping WAF active with optimized traffic rules during peak registration.
*   **Reactive Controls:**
    1. Establishing immediate API key revocation and recycling playbooks.
    2. Activating the corporate Incident Response Plan.
    3. Monitoring and blocking transactions in real-time via behavior baselines.
*   **Consequences:**
    1. Financial liability and severe fraud adjustments from the payment provider.
    2. Reputation fallout, loss of student trust, and potential regulatory lawsuits.

---

## Task 5: Risk Treatment & Framework Mapping

Focusing on remediation for the top three critical risks (R1, R2, R4) using the **NIST SP 800-53** framework:

### R1: Firewall Misconfiguration
*   **Strategy:** Mitigate
*   **Action:** Reconfigure core switch routing logic to isolate the student network and route all cross-VLAN traffic through an internal firewall inspect engine.
*   **NIST Control Mapping:**
    *   `AC-4` - Information Flow Enforcement
    *   `SC-7` - Boundary Protection

### R2: Exposed Frontend API Key
*   **Strategy:** Mitigate
*   **Action:** Strip all credentials from the frontend codebase. Transition storage to secure server-side environment variables and mandate secure peer code reviews prior to production deployment.
*   **NIST Control Mapping:**
    *   `SC-28` - Protection of Information at Rest
    *   `SA-11` - Developer Testing and Evaluation

### R4: Hardened Biometric IoT Setup
*   **Strategy:** Mitigate
*   **Action:** Conduct a bulk update to wipe default factory credentials from all scanners. Re-assign all biometric hardware interfaces to an isolated IoT network segment separate from Student WiFi.
*   **NIST Control Mapping:**
    *   `IA-2` - Identification and Authentication
    *   `SC-39` - Process Isolation
