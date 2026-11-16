# active-directory-homelab
Hands-on cybersecurity homelab featuring Active Directory, Windows Server, Splunk SIEM monitoring, Group Policy, and Ubuntu-based DNS filtering with AdGuard Home.
Active Directory SOC Homelab

Windows Server | Active Directory | Splunk Enterprise | Ubuntu | AdGuard Home

Project Overview

This project demonstrates the deployment of a virtualized Active Directory environment integrated with centralized security monitoring and DNS filtering.

The foundational Windows domain environment was built using a guided Active Directory tutorial and subsequently expanded with Splunk Enterprise for Windows Security event analysis and AdGuard Home for DNS filtering.

The objective was to gain hands-on experience in Windows Server administration, networking, identity management, and SOC investigation techniques.

Technologies Used

Technology

Purpose

Oracle VirtualBox

Virtualization and network configuration

Windows Server

Domain Controller

Active Directory

Domain authentication and user management

Group Policy

Centralized user and system restrictions

DNS / DHCP

Name resolution and IP address allocation

Windows 10

Domain-joined client workstation

Splunk Enterprise

Security log monitoring and analysis

Splunk Universal Forwarder

Windows Security event forwarding

Ubuntu Linux

Hosting DNS security services

AdGuard Home

DNS filtering and query monitoring

Network Architecture

The environment uses an internal VirtualBox network for communication between the Domain Controller, Windows client, and Ubuntu server.

Splunk Enterprise runs on the physical Windows host and receives Security logs from the Domain Controller.

flowchart TB
    Internet((Internet))
    Host["Windows 11 Host<br/>Splunk Enterprise<br/>TCP 9997"]
    DC["Windows Server DC<br/>172.16.0.1<br/>AD DS | DNS | DHCP | GPO"]
    Client["Windows 10 CLIENT1<br/>Domain-Joined Workstation"]
    Ubuntu["Ubuntu Server<br/>172.16.0.101<br/>AdGuard Home"]
    Quad9["Quad9<br/>DNS-over-HTTPS"]

    subgraph VirtualBox["Oracle VirtualBox — Homelab"]
        DC
        Client
        Ubuntu
    end

    Internet --- DC
    Internet --- Ubuntu
    DC <-->|"Domain Authentication / DHCP"| Client
    DC -->|"Windows Security Logs"| Host
    DC -->|"External DNS Queries"| Ubuntu
    Ubuntu -->|"Allowed DNS Queries"| Quad9

Network Configuration

Component

Configuration

Active Directory domain

mydomain.com

Domain Controller

172.16.0.1

DHCP scope

172.16.0.100–172.16.0.200

AdGuard Home

172.16.0.101

Splunk Enterprise

Physical Windows host

1. Active Directory and Windows Server

Configured a Windows Server Domain Controller to provide centralized identity management and essential network services.

Implementation

Installed and configured Active Directory Domain Services.

Created the mydomain.com domain.

Configured DNS and DHCP services.

Joined a Windows 10 workstation to the domain.

Created Active Directory test accounts and Organizational Units.

Configured Group Policy restrictions.

Practiced domain authentication and network troubleshooting.

Screenshots

Domain Controller



Active Directory Users and Organizational Units



Group Policy



DHCP Scope



2. Splunk Enterprise — Security Monitoring

Deployed Splunk Enterprise on the physical Windows host and configured Splunk Universal Forwarder on the Domain Controller to collect Windows Security events.

Implementation

Configured centralized Windows Security log collection.

Verified successful event ingestion from the Domain Controller.

Used Search Processing Language (SPL) to investigate authentication activity.

Examined failed authentication events, including Event IDs 4625 and 4776.

Reviewed account information, timestamps, and authentication failure codes.

Failed Authentication Investigation

Identified Event ID 4625 associated with the a-wbryant account on the Domain Controller.

The event recorded an unsuccessful authentication attempt with an incorrect-password substatus (0xC000006A). The activity was examined as a security-monitoring exercise, not classified as a confirmed attack.

Example SPL Query

index=* sourcetype="WinEventLog:Security" host="DC" EventCode=4625
| table _time host EventCode Message

Screenshots

Centralized Security Log Collection



Failed Authentication Event



SPL Investigation Summary



3. Ubuntu and AdGuard Home — DNS Security

Installed AdGuard Home on an Ubuntu virtual machine to provide DNS filtering and visibility into domain requests.

Implementation

Configured Ubuntu networking and connectivity to the Domain Controller.

Installed and configured AdGuard Home.

Enabled DNS filtering using the AdGuard DNS filter.

Configured Quad9 DNS-over-HTTPS as the upstream resolver.

Configured Windows Server DNS forwarding to AdGuard Home.

Verified DNS query logging and domain blocking.

DNS Filtering Validation

Executed the following DNS lookup:

nslookup doubleclick.net. 172.16.0.101

The AdGuard Home Query Log confirmed that A and AAAA requests for doubleclick.net were blocked by the configured DNS filter.

This demonstrated functioning DNS filtering and monitoring of requests originating from the Domain Controller.

Screenshots

AdGuard Home Dashboard



DNS Blocklist Configuration



Blocked DNS Request



Skills Demonstrated

Windows Server and Active Directory administration

Organizational Unit and user account management

Group Policy configuration

DNS, DHCP, and virtual networking

Windows authentication troubleshooting

Splunk Enterprise and Universal Forwarder configuration

Windows Security event investigation

SPL querying and log analysis

Ubuntu Linux administration

DNS filtering and blocklist management

Technical troubleshooting and documentation

Challenges and Lessons Learned

Building the environment required troubleshooting virtual network connectivity, Windows authentication, Security log visibility, and DNS configuration.

The Splunk investigation reinforced the importance of identifying where authentication events are recorded and distinguishing between different user accounts.

Configuring AdGuard Home also provided experience resolving DNS service conflicts and validating filtering through actual DNS queries.

Conclusion

This project provided hands-on experience configuring and troubleshooting a Windows domain environment while integrating centralized security monitoring and Linux-based DNS filtering.

The completed homelab demonstrates foundational technical skills relevant to IT support, systems administration, and entry-level cybersecurity roles.
