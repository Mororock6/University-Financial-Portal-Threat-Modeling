# 🛡️ University Financial Portal - Threat Modeling Project

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) This repository presents a comprehensive threat model for a University Financial Portal, designed using Microsoft Threat Modeling Tool (TMT). The project evaluates potential risks and proposes mitigation strategies to secure the communication, authentication, and data flow across student and staff user environments.

## 🧩 Project Overview

The system allows students and internal staff to securely interact with a financial server through a web portal:

* **Students:** Authenticate using a dedicated Active Directory (AD) domain within a Demilitarized Zone (DMZ) network segment.
* **Internal Staff:** Access the system through a Virtual Private Network (VPN) or a secure internal network.

The threat model focuses on the architecture and potential vulnerabilities associated with the new student web portal and its interaction with the existing financial system.

## 🔐 System Features & Security Objectives (Based on Requirements)

1.  **Student Web Portal Implementation:**
    * Secure web-based portal for university students.
    * Secure communication over HTTPS (no VPN required for students).
    * Student authentication via a separate Active Directory (AD) domain.

2.  **Student Portal Functionalities:**
    * View current balance.
    * Access payment history.
    * Set up payment accounts.
    * Make one-time payments.
    * Set up automatic payments.

3.  **Existing Financial System Features:**
    * Staff access via internal network or secure web portal (VPN required).
    * Encrypted internal network traffic between user workstations, application servers, and databases.
    * Encrypted database records for sensitive data.
    * Staff authentication via Active Directory.
    * Application access control using AD Security Group Policy Objects (GPOs).

4.  **Technical Setup:**
    * Web portal server hosted within the data center's DMZ.
    * Encrypted connections between the web portal and internal resources (domain controllers and other servers).
    * Dedicated Student AD Domain Controller within the DMZ for student credential validation.

## 🧠 Threat Modeling (Based on TMT Diagrams)

Conducted using Microsoft Threat Modeling Tool. The diagrams illustrate the data flow and security boundaries within the system. Key components analyzed include:

* **Student User Interaction:** Accessing the web portal through browsers.
* **Proxy Server:** Handling student requests and potentially providing a layer of security.
* **Student AD Domain Controller:** Authenticating student users.
* **Financial Server:** Hosting sensitive financial data and business logic.
* **Internal Staff Interaction:** Accessing the financial system via internal network or VPN.
* **Internal AD:** Managing staff authentication and authorization.
* **Authorization Provider:** Likely involved in controlling access to financial resources.
* **Web Server Cache:** Potentially storing temporary data.
* **Cookies:** Used for session management.
* **Databases:** Storing financial data and potentially user information.

**Identified Potential Threat Areas (Based on Image 1 - Fault Tree Analysis):**

The fault tree analysis in Image 1 focuses on the high-level goal of "Steal Financial Data" and outlines potential attack paths:

* **Hack Financial Database:**
    * **Authorized Access:** Malicious internal staff.
    * **Unauthorized Access:**
        * Bypassing Authentication (e.g., Stolen Cookie, Leaked Username/Password).
        * Physical Access (Direct Access to workstations, Left-open Workstations, Directly installing malware).
* **Intercept Communication:**
    * Phishing attacks.
    * SSL/TLS Stripping (Downgrading encryption).
    * Packet Sniffing (Capturing network traffic).
        * Connecting from an infected device.
        * Connecting through an unsafe network/public Wi-Fi.

**Identified Key Data Flows and Components (Based on Image 2 - Data Flow Diagram):**

The Data Flow Diagram (DFD) in Image 2 provides a detailed view of the system's components and how data moves between them. Key elements include:

* **External Entities:** Students and Internal Staff.
* **Processes:** Web Client (Student Browser), Proxy Server, Student AD, Financial Server, Internal AD, Authorization Provider, IT Management Server.
* **Data Stores:** Cookies (Student & Staff), Web Server Cache, Financial Database, AD Databases (Student & Internal).
* **Data Flows:** Depicting requests, responses, authentication data, financial data, and management information.
* **Security Boundaries:** Clearly demarcating the University Network, Internal Network, and DMZ.

**Observed Security Measures (Based on Requirements and Diagrams):**

* Use of HTTPS for student portal communication.
* Separate AD domains for students and staff.
* Placement of the web portal in the DMZ.
* Encrypted connections between web portal and internal resources.
* VPN requirement for staff accessing the portal externally.
* Encryption of internal network traffic and database records.
* Use of firewalls (Internal and External are implied by the network boundaries).
* Proxy server for the student portal.

## 🛡️ Potential Threats & Mitigation Strategies (To be expanded)

Based on the analysis, here are some potential threats and initial mitigation thoughts. This section can be further expanded as you delve deeper into the threat model.

| Threat Category          | Specific Threat                                   | Potential Mitigation Strategies                                                                                                                               |
| :----------------------- | :------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Authentication** | Brute-force attacks on student portal login      | Implement strong password policies, account lockout mechanisms, and potentially multi-factor authentication in the future.                                  |
|                          | Credential stuffing attacks                       | Monitor for suspicious login attempts, consider rate limiting, and educate users on password security.                                                      |
|                          | Session hijacking (Stolen Cookies)              | Implement secure cookie handling (HTTPOnly, Secure flags), short session timeouts, and potentially session invalidation upon suspicious activity.         |
| **Access Control** | Unauthorized access to financial data by students | Implement robust authorization checks on the Financial Server, ensuring students can only access their own data. Principle of least privilege.         |
|                          | Privilege escalation by malicious internal staff | Enforce strict role-based access control (RBAC) and regularly audit user permissions and activity.                                                           |
| **Network Security** | Man-in-the-Middle (MitM) attacks on student portal | Ensure proper TLS/SSL configuration on the web server, educate users about the risks of public Wi-Fi.                                                    |
|                          | Attacks targeting the DMZ infrastructure          | Harden the web portal server and Student AD, implement strong firewall rules, and regularly patch systems. Intrusion Detection/Prevention Systems (IDS/IPS). |
| **Data Security** | SQL Injection attacks against the Financial DB    | Implement secure coding practices, use parameterized queries or prepared statements, and perform regular security testing.                                 |
|                          | Data breaches due to compromised servers         | Implement strong encryption for data at rest and in transit, enforce strict access controls, and have robust incident response plans.                     |
| **Physical Security** | Unauthorized physical access to servers          | Implement strong physical security measures for the data center.                                                                                             |
| **Malware** | Malware infection leading to data theft          | Implement endpoint protection, user education on phishing and malicious attachments, and network segmentation.                                             |

## 📁 Project Contents

* `TMT_Diagram_Image1.png`: Screenshot of the Fault Tree Analysis from Microsoft Threat Modeling Tool.
* `TMT_Diagram_Image2.png`: Screenshot of the Data Flow Diagram from Microsoft Threat Modeling Tool.
* `Report for TMT.htm`: The full threat modeling report generated by the Microsoft Threat Modeling Tool (if you include it).
* `README.md`: This documentation file.
* `LICENSE` (Optional): If you choose to include a license.

## 🚀 Getting Started

1.  Clone this repository to your local machine:
    ```bash
    git clone <repository-url>
    ```
2.  Review the threat model diagrams and the full report (if included) to understand the identified threats and proposed mitigations in detail.
3.  Contribute to the mitigation strategies section or suggest improvements by creating pull requests.

## 🤝 Contributions

Contributions to enhance this threat model or suggest better mitigation strategies are welcome. Please feel free to fork the repository and submit pull requests.

## 📄 License

[Specify your license here, e.g., MIT License](https://opensource.org/licenses/MIT) (Or remove this section if you don't want to specify a license).

---

**Step 3: Commit and Push**

1.  Save the `README.md` file (and any other files like the TMT report and image files) in your local directory where you cloned the repository.
2.  Open your terminal or command prompt, navigate to the repository directory, and run the following commands:
    ```bash
    git add .
    git commit -m "Initial commit: Adding threat model documentation and diagrams"
    git push origin main
    ```
    (If your main branch is named something else like `master`, replace `main` accordingly).

**Important Considerations and Potential Updates:**

* **Threat Mitigation Strategies:** The table in the README provides initial thoughts. You should expand this significantly based on the detailed findings in your TMT report. Include specific technical controls and processes that can mitigate the identified threats.
* **TMT Report:** If the `Report for TMT.htm` file contains detailed information about threats, vulnerabilities, and suggested mitigations, consider including it in the repository. You might also want to extract key findings from it and summarize them more concisely in the README.
* **Diagram Clarity:** Ensure the screenshots of your TMT diagrams are clear and easy to understand. If possible, consider exporting them in a higher resolution or as a different image format if it improves readability.
* **Further Analysis:** As you continue to work on this project, you might identify new threats or refine your mitigation strategies. Keep the README updated to reflect the latest state of your threat model.
* **Consider a Dedicated Threat Report File:** If the mitigation strategies become extensive, you might want to create a separate document (e.g., `Threat_Report.md` or `Threat_Report.docx`) to keep the README concise.

By following these steps, you'll have a well-structured and informative GitHub repository for your University Financial Portal threat modeling project. Remember to continuously update it as your understanding evolves. Let me know if you have any other questions!
