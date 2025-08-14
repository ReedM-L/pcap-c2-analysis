# PCAP Analysis: C2 and Suspicious Activity Investigation

## Overview
This analysis examines a PCAP file capturing network traffic from an infected host.  

**Objective:** Identify phishing activity and potential Command-and-Control (C2) servers.  
  
**Infected host IP:** `10.1.17.215`  
**Infected host MAC:** `00:d0:b7:26:4a:74`

## Lab Setup / Environment
- Tools: Wireshark `4.4.6`  
- Network briefing: `Infected client was directed to a phishing site posing as a Google authenticator site which started the infection. LAN segment range 10.1.17.0/24. Active Directory (AD) environment name: BlUEMOONTUESDAY`
![Screenshot](./images/Description.png)
*Figure 1: PCAP lab information background, details, and tasks*
## Steps Taken

### 1. Identify Infected Host
- Filtered traffic by dns
- Looked for DNS queries to domains that may be typosquatting or impersonating well-known websites based upon the background information given
- Found DNS queries to "authenticatoor.org" concluded that this must be the infected host (10.1.17.215)
![Screenshot](./images/DNSfilter.png)
*Figure 2: Suspicious domain found through DNS filter*

### 2. Find Info on Infected Host
- Inspected infected host to gather their domain name, MAC address, and username
- Found domain name through NetBios Name Service (NBNS), username through Kerberos, and MAC address was found in both of these places as well.
- ![Screenshot](./images/domainName.png) ![Screenshot](./images/Username-client.png)

### 3. Inspect Outbound Traffic for C2
- Filtered packets going to the infected host
- Applied packet size filters to highlight potential C2 traffic
- Was keeping an eye out for continous, quick, and small traffic
- ![Screenshot](./images/C2Server.png)

### 4. Identify Candidate C2 Servers
- Reviewed external IPs/domains contacted repeatedly  
- Used Wireshark → Statistics → Conversations → IPv4 to confirm top destinations  
- Likely C2 servers identified:  
  - `45.125.66.252`

## Observations
- High-frequency TLS traffic to external server  
- Repetitive small packets, indicative of potential C2 communication  
- Connections to external domains unrelated to normal browsing  
- Indicators of compromise (IoCs) noted  

## Conclusion
- Infected host identified and monitored  
- User was directed to a fake authentication domain: `authenticatoor.org`  
- One likely C2 servers detected: `45.125.66.252`  
- Patterns suggest automated communication with C2 infrastructure


## Lessons Learned / Next Steps
- Always cut out as much "noise" as possible 
- Analyze logs for potential data exfiltration  
- Consider automated detection tools for C2 traffic monitoring
- Briefing mentioned multiple C2 servers, learn about C2 traffic and identify any additonal servers that contributed to C2 attack on infected host
