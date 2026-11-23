# Wazuh SIEM Homelab

This repository contains a simple SIEM homelab utilizing two ubuntu VMs (Wazuh server and agent), VirusTotal API, and a python script. The main goals of the SIEM are as follows:
- Understand the basics of configuring multiple Virtual Machines using VMware Workstation Pro
- Establish Wazuh's File Integrity Monitoring (FIM) features
- Utilize VirusTotal's real time malware detection
- Automatically delete files marked by VirusTotal through the use of a python script.

## Architecture

```mermaid
flowchart TD

subgraph group_endpoint["Endpoint Monitoring"]
  node_agent["Wazuh Agent<br/>[agent.conf]"]
  node_fim["File Integrity Monitoring<br/>[agent.conf]"]
end

subgraph group_detection["Threat Detection"]
  node_server["Wazuh Server<br/>[ossec.conf]"]
  node_virustotal{{"VirusTotal API"}}
end

subgraph group_response["Automated Response"]
  node_deleter["Python Deletion Script"]
end

node_files["Monitored Files"]

node_files -->|"monitored by"| node_agent
node_agent -->|"uses FIM"| node_fim
node_server -->|"uses detection"| node_virustotal
node_virustotal -->|"marks files"| node_deleter
node_deleter -->|"deletes marked"| node_files

click node_agent "https://github.com/michaelbosaz/homelab-siem/blob/main/configs/agent.conf"
click node_fim "https://github.com/michaelbosaz/homelab-siem/blob/main/configs/agent.conf"
click node_server "https://github.com/michaelbosaz/homelab-siem/blob/main/configs/ossec.conf"

classDef toneNeutral fill:#f8fafc,stroke:#334155,stroke-width:1.5px,color:#0f172a
classDef toneBlue fill:#dbeafe,stroke:#2563eb,stroke-width:1.5px,color:#172554
classDef toneAmber fill:#fef3c7,stroke:#d97706,stroke-width:1.5px,color:#78350f
classDef toneMint fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px,color:#14532d
classDef toneRose fill:#ffe4e6,stroke:#e11d48,stroke-width:1.5px,color:#881337
classDef toneIndigo fill:#e0e7ff,stroke:#4f46e5,stroke-width:1.5px,color:#312e81
classDef toneTeal fill:#ccfbf1,stroke:#0f766e,stroke-width:1.5px,color:#134e4a
class node_agent,node_fim toneBlue
class node_server,node_virustotal toneAmber
class node_deleter toneMint
class node_files toneIndigo
```
