```mermaid
flowchart LR

subgraph NET["VMware NAT Network (192.168.59.0/24)"]

W10["Windows 10 Endpoint
192.168.59.162

• Universal Forwarder
• Atomic Red Team
• Claude Desktop
• Splunk MCP Client"]

SPL["Splunk Enterprise
Ubuntu VM
192.168.59.157

• Log Ingestion
• Searches
• Alerts
• Detection Rules"]

N8N["n8n Automation Server
Ubuntu VM
192.168.59.159

• Webhooks
• Workflows
• AI Processing
• Enrichment"]

IRIS["DFIR-IRIS
Ubuntu VM
192.168.59.160

• Case Management
• Incident Tracking"]

end

OAI["OpenAI API"]
VT["VirusTotal"]
AB["AbuseIPDB"]
SLACK["Slack Cloud"]

W10 -->|"Windows Logs / Telemetry"| SPL
W10 -->|"Attack Simulation"| SPL

SPL -->|"Alerts / Webhooks"| N8N

N8N -->|"IOC Enrichment"| VT
N8N -->|"IP Reputation"| AB
N8N -->|"AI Summary / Triage"| OAI

N8N -->|"Create Incident"| IRIS
N8N -->|"SOC Notification"| SLACK

W10 -->|"Claude + MCP Queries"| SPL
