
 

# Team Winix Technical Report 

## Team Submission 

## Term Project CST8248 

#### Submitted 2020-12-03 

 

 

#### Team Members: 

Brendan Marentette 

Nicholas Hancin 

Kolawole Adamo 

 

Table of Contents 

 

 

Section to be added once other parts of the document are done. Table of contents will include: 

Major Headings 

Minor Headings 

List of Figures, Tables or Charts (Unless you decide to include a Table of Figures) 

Page Numbers 
 

Our document will include the following sections:
 

Title Page 

Table of Contents 

Executive Summary 

Introduction 

Background 

Discussion  

Conclusion 

Appendices 

 
### Executive Summary 

 

Executive Summary will include the following: 

Short summary 

Should detail the key points for decision making purposes. 

Consider the purpose of your document while writing the executive summary 

 

 

 

 

### Introduction: 

Intro will include the following: 

Introduce the document 

Describe the purpose of the document 

Describe the main content 

Scope of the project 

 

 

 

 

### Background 

 

This document is intended to be read by the Computer System Technicians (CSTs) working for our client, Winix Laboratories Inc. The document will describe configuration decisions and other details that are intended to assist the CSTs with maintaining the services.  

For most of the services, individual servers are configured to perform specific tasks, such as the SMB file server on machine fileserve.winix.lab. Service stacking is limiting to the Spiceworks, DNS and Veeam services for reasons described later in this document. The use of individual systems tasked with one service is intended to minimize the complexity of server maintenance and facilitate easier backups and restores. This “modular” service design also aids in the creation of additional secondary or tertiary services as needed. Some of the critical services, such as ADDS, DNS and NTP already have secondary servers that can be used in the event of a failure on the primary systems.  

 

The Background will include: 

What does the reader need to understand before exploring this document further 

Who is the intended reader 

 

 

 

### Discussion 

The Discussion is expected to include the following: 

Main body of your document.   

Which services and platforms & why. 

Which networks and IP addresses & why 

Are there limitations/ non-functioning equipment. 

What maintenance/Daily Duties  

What are your thoughts on the matter. 

Why does your reader care. 

 

In this section, we should include 1 page per service that we’ve set up. The page should consist of at least two paragraphs: 

one describing the service and how it was set up (as described in the points above).  

One describing any specific config decisions we made and why (eg. Why I used MD5 authentication for NTP) 

 
***
#### NTP
The NTP service was configured on ntp.winix.lab. This service was chosen as one of the value added deliverables for the project
in order to ensure effective time synchronization between all of the servers on the back end network. NTP was configured on a 
CentOS 7 server using the "ntpd" service. This service was chosen for its high degree of reliability and simplicity of maintenance.
The only ongoing maintenance needed on this server will be patches and updates. The server was configured to use encryption to 
validate machines that attempt to connect to the service. The encryption is intended to harden the server against attacks against
the NTP service or attacks that misuse the NTP service. In addition, the NTP server is configured to only provide the service on
the LAN. This further protects the server from misuse or comprimise.


#### HTTP / HTTPS
The HTTP and HTTPS services were configured on webserve.winix.lab. This service is configured to function only on the LAN; as a result,
any users wishing to connect to this server and its websites must be connected on the Blue network. The web server runs on CentOS 7 and
the chosen server software is Apache. Apache was chosen for its ease of maintenance and the auditable nature of it as open source software.
Both of the websites available on this server were only configured with basic landing pages that function as a home page. The webpages are
accessible at the following URLs on the LAN: http://http.winix.lab and https://secure.winix.lab. The secure website is configured using SSL
to protect connections between clients and the server. The certificate used is a self-signed certificate. This configuration decision was
made because HTTPS will provide a considerably higher degree of security than the basic HTTP website. 


#### SMB
The file server on the network utilizes the SMB protocol running on the server fileserve.winix.lab. The virtual machine operating system in
use is CentOS 7 and the file system software is Samba (running as the daemon "smbd"). This Operating System and server software was chosen
because of the ease of hardening and open source roots. The server is configured to use the Domain accounts for authorization and 
authentication. This design decision was made in order to maximize the simplicity and security of the service and minimize the need to have
separate Samba login information for each user that requires access to the file server. The file share is accessible on the Blue network
at \\fileserve.winix.lab\WinixFiles.


#### Firewalls
The firewalls in use on the network are differentiated between the Operating Systems in use: Windows Server 2016/2019 and CentOS 7. The Windows
Server firewall rules are configured using the Windows Firewall utility. The firewall rules are configured to permit the establishment of
connections that are necessary for the server to work effectively, while also denying any traffic that does not need to be permitted to connect
with the server. For example, the DNS server is configured to allow DNS queries and RDP traffic on the local subnet, but does not permit any 
uneeded traffic because our firewall policy makes use of a defined "deny all" rule in order to disallow unnecessary connections or traffic.
The firewall service in use on the Linux machines is IPTables. This service was chosen because the servers required a stateful firewall 
configuration in order to provide the desired level of protection. The IPTables rules on the machines are similar across all of the CentOS 
servers, with the main difference being the specific port connections that are permitted. For example, all servers are set up to permit
HTTP and DNS traffic (which are needed for Spiceworks to function) and all of the servers have an explicit "deny all" statement for inbound
and outbound connections that aren't specified in the rules. Refer to Appendix 1 to see an example of a firewall configuration that is in 
use on one of the virtual machines.


#### Active Directory



#### DNS



#### Firewalls



#### Spiceworks



#### iSCSI



#### Firewalls
 


#### Windows Admin Centre



#### Azure Portal

 

The Conclusion will include: 

What was learned. 

Summarize document 

DO NOT introduce new content here!!! 

This is the end of the content/writing section. 

 

 

 

 

 

### Appendices 

 

The Appendices can include the following: 

Stuff that does not fit in the body of the document 

Config files go well here 

Unresolved troubleshooting  

We probably won’t have much info to include here, so we should copy/paste our config files here so that we have enough content. We will also include our test plan in the appendices. 

#### Appendix 1) Sample Firewall Configuration

- IPTABLES RULES for fileserve.winix.lab

- Rules for loopback interface
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

- Rule to permit established and related incoming connections that the server establishes
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

- Rule to permit established outgoing connections
iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED -j ACCEPT

- Rules for Samba service
iptables -A INPUT -s 172.20.80.0/24 -m conntrack --ctstate NEW -p udp --dport 137 -j ACCEPT
iptables -A INPUT -s 172.20.80.0/24 -m conntrack --ctstate NEW -p udp --dport 138 -j ACCEPT
iptables -A INPUT -s 172.20.80.0/24 -m conntrack --ctstate NEW -p tcp --dport 139 -j ACCEPT
iptables -A INPUT -s 172.20.80.0/24 -m conntrack --ctstate NEW -p tcp --dport 445 -j ACCEPT
iptables -A INPUT -s 172.20.80.0/24 -m conntrack --ctstate NEW -p tcp --dport 631 -j ACCEPT

- Rules for SSH service
iptables -A INPUT -s 172.20.80.0/24 -m conntrack --ctstate NEW,ESTABLISHED -p tcp --dport 22 -j ACCEPT
iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED -p tcp --sport 22 -j ACCEPT
iptables -A OUTPUT -m conntrack --ctstate NEW,ESTABLISHED -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED -p tcp --sport 22 -j ACCEPT

- Rules for HTTP and HTTPS
iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT
iptables -A OUTPUT -p udp --dport 80 -j ACCEPT
iptables -A OUTPUT -p tcp --dport 443 -j ACCEPT
iptables -A OUTPUT -p udp --dport 443 -j ACCEPT

- Rules for Spiceworks
iptables -A INPUT -m conntrack --ctstate NEW -p tcp --dport 9675 -j ACCEPT
iptables -A INPUT -m conntrack --ctstate NEW -p tcp --dport 9676 -j ACCEPT
iptables -A INPUT -m conntrack --ctstate NEW -p udp --dport 9675 -j ACCEPT
iptables -A INPUT -m conntrack --ctstate NEW -p udp --dport 9676 -j ACCEPT

- Rules for DNS
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
iptables -A INPUT -p udp --sport 53 -j ACCEPT

- log before dropping packet
iptables -A INPUT -j LOG
iptables -A INPUT -j LOG

- Set all major commands to DROP by default
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP

 

 

 

### References 

 

Please copy/paste the links to any sites/sources you used. Brendan can convert the links and ISBNs into formatted IEEE references. 

 
