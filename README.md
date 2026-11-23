# Wazuh SIEM Homelab

This repository contains a simple SIEM homelab utilizing two ubuntu VMs (Wazuh server and agent), VirusTotal API, and a python script. The main goals of the SIEM are as follows:
- Understand the basics of configuring multiple Virtual Machines using VMware Workstation Pro
- Establish Wazuh's File Integrity Monitoring (FIM) features
- Utilize VirusTotal's real time malware detection
- Automatically delete files marked by VirusTotal through the use of a python script.

## Architecture

```mermaid
flowchart TD

subgraph group_deployment["Wazuh setup"]
  node_server["Wazuh server<br/>[ossec.conf]"]
  node_agent["Wazuh agent<br/>[agent.conf]"]
end

subgraph group_detection["Threat detection"]
  node_fim["File integrity monitoring<br/>[agent.conf]"]
  node_virustotal["VirusTotal API"]
end

subgraph group_response["Automated response"]
  node_response_script["Deletion script"]
  node_flagged_files["Flagged files"]
end

node_operator(("Homelab operator"))

node_operator -.->|"configures"| node_server
node_operator -.->|"configures"| node_agent
node_agent -.->|"monitors"| node_fim
node_virustotal -.->|"marks"| node_flagged_files
node_response_script -.->|"deletes"| node_flagged_files

click node_server "https://github.com/michaelbosaz/siem-homelab/blob/main/configs/ossec.conf"
click node_agent "https://github.com/michaelbosaz/siem-homelab/blob/main/configs/agent.conf"
click node_fim "https://github.com/michaelbosaz/siem-homelab/blob/main/configs/agent.conf"

classDef toneNeutral fill:#f8fafc,stroke:#334155,stroke-width:1.5px,color:#0f172a
classDef toneBlue fill:#dbeafe,stroke:#2563eb,stroke-width:1.5px,color:#172554
classDef toneAmber fill:#fef3c7,stroke:#d97706,stroke-width:1.5px,color:#78350f
classDef toneMint fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px,color:#14532d
classDef toneRose fill:#ffe4e6,stroke:#e11d48,stroke-width:1.5px,color:#881337
classDef toneIndigo fill:#e0e7ff,stroke:#4f46e5,stroke-width:1.5px,color:#312e81
classDef toneTeal fill:#ccfbf1,stroke:#0f766e,stroke-width:1.5px,color:#134e4a
class node_server,node_agent toneBlue
class node_fim,node_virustotal toneAmber
class node_response_script,node_flagged_files toneMint
class node_operator toneIndigo
```
## Configuration

### File Integrity Monitoring:
- Detects file creation, deletion and changes
- The Ubuntu agent was set up to detect changes in the entire endpoint system
  
### VirusTotal:
- Integrated into Wazuh allowing communication between VirusTotal and the Wazuh SIEM
- VirusTotal can generate alerts based on the config rules
- Alerts are used in Active Response Deletion

### Active Response:
- Through the utilization of the unmodified script found in Wazuh Documentation: [Detecting and removing malware using VirusTotal](https://documentation.wazuh.com/current/proof-of-concept-guide/detect-remove-malware-virustotal.html) I was able to establish as system that deletes detected malicious files
- The script deletes files that are flagged by the VirusTotal API

## Learning Takeaways
This SIEM home lab introduces foundational cybersecurity concepts relevent to SOC analysts.

- Creating and understanding dashboards that organize data
- Integrating python scripts for preventing malware attempts and establishing active response
- Identifying threats using Wazuh
- Setting up Ubuntu VMs
  
## Future Improvements
- Add a windows endpoint to explore the use of multiple operating systems in one SIEM
- Add a Kali VM in order to generate real attacks to see how they appear in the log
- Add an allowlist with the goal of reducing false positives
