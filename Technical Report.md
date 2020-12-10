
 

# Team Winix Technical Report 

## Team Submission 

## Term Project CST8248 

#### Submitted 2020-12-10

 

 

### Team Members: 

Brendan Marentette 

Nicholas Hancin 

Kolawole Adamo 

***
 
### Table of Contents 

1) Executive Summary 

2) Introduction 

3) Background 

4) Discussion  

5) Conclusion 

6) Appendices 

 
### Executive Summary 

This Technical Project will describe the decision making and implementation factors that resulted in the creation of the services
delivered in this project. Overall, the main considerations used to dictate service design are service segmentation (ie. avoiding
service stacking) and ease of maintenance. This document is intended to function as a high level description of the individual 
services to be used to guide the client in maintaining the services when the servers are transferred to the client.

 

### Introduction: 

The project presented for your consideration includes a variety of services that are intended to provide a comprehensive set of Information Technology (IT)
resources for an office owned by the client, Winix Labratories Inc. The services available on the network include: DNS, Samba, NTP, iSCSI, Veeam, Spiceworks,
and some cloud services hosted in Microsoft Azure. The overall design of the deliverables and their individuals configurations are intended to optimize the 
VMs to be easy to maintain and simple to reconfigure and alter as desired. The main content of this document will include brief summaries of the configurations and decisions that were made when setting up the individual servers.
 

### Background 

This document is intended to be read by the Computer System Technicians (CSTs) working for our client, Winix Laboratories Inc. The document will describe configuration decisions and other details that are intended to assist the CSTs with maintaining the services.  

For most of the services, individual servers are configured to perform specific tasks, such as the SMB file server on machine fileserve.winix.lab. Service stacking is limiting to the Spiceworks, DNS and Veeam services for reasons described later in this document. The use of individual systems tasked with one service is intended to minimize the complexity of server maintenance and facilitate easier backups and restores. This “modular” service design also aids in the creation of additional secondary or tertiary services as needed. Some of the critical services, such as ADDS, DNS and NTP already have secondary servers that can be used in the event of a failure on the primary systems. Supplementary information about the software and hardware configuration of the services and servers can be found in the Progress Log included in this document (refer to Appendix 1). In addition, a sample of our Testing Plan is included in Appendix 2.


### Discussion 

#### Active Directory
The active directory is essential for businesses to work. A service where you can implement security, permissions in a windows environment which act as a single sign-up with access to resources.
The Active Directory is the domain controller that stores all information of the users and computers account in the domain controller. So, for each of the servers used to manage different services they are added to the domain controller, and a secondary domain controller is advised to be configured, in support for the master DC. If down the second act in support for continuity of service. The operating system used is the Windows server 2019 and, this service was chosen, because of how it can handle a small to a large number of users and computers. In the Active Directory server the DNS is installed to be able to maps IP addresses to a fully qualified domain name (FQDN), and using the inbuilt DNS services is much simpler to use, which saves time to configure. These services are configured to manage a small environment but if in a large environment the active directory and the DNS services would be on a separate server for easy use and access depending on the hardware specification.
<image fot active directory>


#### DNS
The primary and secondary DNS servers were originally configured on CentOS using the Named DNS service. However, it was later determined that
the service should be implemented on a Windows Server in order to ensure that the configuration would be compatible with the services that were
installed later (such as Spiceworks). As a result, the final version of the DNS servers are running on the primary and secondary Domain Controllers.
As a result, the DNS service is perfectly compatible with all services and will be easy to maintain by the client. In its current state, the DNS
servers function in a master/slave relationship between DC01 and DC02. This configuration was set up in order to ensure that the DNS service is 
fault tolerant in the event of DC01 experiencing a failure.


#### Firewalls
The host-based firewalls in use on the network are differentiated between the Operating Systems in use: Windows Server 2016/2019 and CentOS 7. The Windows
Server firewall rules are configured using the Windows Firewall utility. The firewall rules are configured to permit the establishment of
connections that are necessary for the server to work effectively, while also denying any traffic that does not need to be permitted to connect
with the server. For example, the DNS server is configured to allow DNS queries and RDP traffic on the local subnet, but does not permit any 
uneeded traffic because our firewall policy makes use of a defined "deny all" rule in order to disallow unnecessary connections or traffic.
The firewall service in use on the Linux machines is IPTables. This service was chosen because the servers required a stateful firewall 
configuration in order to provide the desired level of protection. The IPTables rules on the machines are similar across all of the CentOS 
servers, with the main difference being the specific port connections that are permitted. For example, all servers are set up to permit
HTTP and DNS traffic (which are needed for Spiceworks to function) and all of the servers have an explicit "deny all" statement for inbound
and outbound connections that aren't specified in the rules. Refer to Appendix 3 to see an example of a firewall configuration that is in 
use on one of the virtual machines.


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

![Table1](https://github.com/hanc0035/Winix/blob/master/images/%251.PNG)


#### SMB
The file server on the network utilizes the SMB protocol running on the server fileserve.winix.lab. The virtual machine operating system in
use is CentOS 7 and the file system software is Samba (running as the daemon "smbd"). This Operating System and server software was chosen
because of the ease of hardening and open source roots. The server is configured to use the Domain accounts for authorization and 
authentication. This design decision was made in order to maximize the simplicity and security of the service and minimize the need to have
separate Samba login information for each user that requires access to the file server. The file share is accessible on the Blue network
at \\\fileserve.winix.lab\WinixFiles.

![Table](https://github.com/hanc0035/Winix/blob/master/images/%252.PNG)



#### Inventory
The Inventory Server is running from the server inventory.winix.lab.  The vm is running on Windows Server 2019 and the service used is Spiceworks. The software was chosen because it was the example service for the inventory server. The OS was chosen because it is what was specified by spiceworks setup instructions. The vm uses 2 virtual socket with 2 cores and 6 GB of RAM as specified by spiceworks to run smoothly. The web client for spiceworks is running on localhost and can only be accessed by rdp to the inventory server. The inventory server logs all the systems on the winix.lab domain and log the names of the servers, the OS, and more. This server will notify the user when a system goes down.

![Table](https://github.com/hanc0035/Winix/blob/master/images/spiceworks.png)

#### Backup
The Backup Server is running from the server backup.winix.lab. The vm is running from Windows Server 2019 and the service used for the backup is Veeam backup and replication. The software was chosen because it was the example service for the backup server. The OS was chosen because it was specified by Veeam backup and replication. The vm uses 2 virtual socket and 2 cores and 6 GB of RAM as specified to run smoothly. Veeam is configure to run from any server that has Veeam installed using backup.winix.lab as the backup server. It is setup to backup the http server manually but rules can be added to make the backup automatic. This server will notify the user when space is running low.

#### iSCSI
The ISCSI which uses an IP address connected to the blue network is used to transfers data between the initiator on a server and to the target on another server for storage on a device. This will provide high availability between server and server and it is configured to be scalable and maximize storage.

iSCSI initiator and iSCSI target both connected
<image for ISCSI>
 


#### Windows Admin Centre
This is a locally deployed running from the server wac.winix.lab. A browser-based application for managing windows servers, clusters, infrastructures. In this project, we have used the WAC for other windows server management. Instead of opening the various tools on each of those server operating windows servers. We can now use the windows admin server, which is also considered a lightweight management system for small to large scale deployments. There are familiar MMC tools functionalities.

![Table](https://github.com/hanc0035/Winix/blob/master/images/wac1.png)
 
![Table](https://github.com/hanc0035/Winix/blob/master/images/wac2.png)

Instead of remoting into a server via RDP or other inbox tools, you can manage servers remotely from the browser using the windows admin center.

![Table](https://github.com/hanc0035/Winix/blob/master/images/wac3.png)

![Table](https://github.com/hanc0035/Winix/blob/master/images/wac4.png)
 
There are remote server administration tools for managing servers' roles and features. The windows admin center installs and runs services such as DNS, Active Directory, ISCSI initiator, and ISCSI Target as done in this project. And using one server for each of these services and having a secondary domain controller was the best practice to implement. Just in case one service or a breakdown of the server occurs, it does not affect all services, and if the master domain controller is down, the secondary is there for backup. Overall the admin center oversees the configurations of all servers within its environment.




#### Azure Portal
##### Web Server
The Azure Web Server is configure through Azure's Services. It is accessible anywhere at the address 104.211.0.91. It was intended to be the interface for the PostgreSQL server but due some issues with postmaster, it is currently running a placeholder website. to modify the site, you can connect via SSH.

![Table](https://github.com/hanc0035/Winix/blob/master/images/Azure%20Webserver.png)

##### PostgreSQL 
The Azure database is running PostgreSQL. It is not currently functional as we have come into some issues with postmaster. It is to be the database that uses the webserver as its interface. To modify any settings, it is only accessible via SSH at the ip 13.90.78.8. It is currently a work in progress.

### Conclusion

Overall, this document was intended to provide a high level overview of the setup and configuration decisions that went into the final services 
presented to the customer. The services are configured with maintenance in mind and the avoidance of service stacking should effectively help
to ensure that the goal is achievable. The document has covered basic configuration information about the individual servers, in addition to 
some information about decisions that were made and later reversed due to compatibility limitations or other challenges. Some of the areas where
changes were needed include DNS implementation and challenges arising from problems with the cloud database installation in Microsoft Azure. 

 

### Appendices 

#### Appendix 1) Progress Log

| Hostname              | Completion  | Testing     | Scheduled Start Date | Scheduled Due Date | Responsible Team Member/s | Operating System    | Service                                                       | CPU Sockets | CPU Cores per Socket | Total Cores | RAM \(GB\) | Red Network IP   | Blue Network IP  | Notes                                                                          |
|-----------------------|-------------|-------------|----------------------|--------------------|---------------------------|---------------------|---------------------------------------------------------------|-------------|----------------------|-------------|------------|------------------|------------------|--------------------------------------------------------------------------------|
| ns1\.winix\.lab       | Not in use  | In Progress | 10/9/2020            | 10/16/2020         | Brendan                   | CentOS 7            | Master DNS                                                    | 2           | 2                    | 4           | 4          | 172\.30\.80\.1   | 172\.20\.80\.1   | Not in use; working, but replaced with dc1/dc2 DNS due to compatibility issues |
| ns2\.winix\.lab       | Not in use  | In Progress | 10/9/2020            | 10/16/2020         | Brendan                   | CentOS 7            | Slave DNS                                                     | 1           | 2                    | 2           | 3          | 172\.30\.80\.2   | 172\.20\.80\.2   | Not in use; working, but replaced with dc1/dc2 DNS due to compatibility issues |
| fserver\.winix\.lab   | Complete    | Complete    | 10/16/2020           | 10/23/2020         | Brendan                   | CentOS 7            | Samba Server                                                  | 1           | 2                    | 2           | 3          | 172\.30\.80\.3   | 172\.20\.80\.3   |                                                                                |
| webserve\.winix\.lab  | Complete    | Complete    | 10/17/2020           | 10/24/2020         | Brendan                   | CentOS 7            | HTTP/HTTPS Server                                             | 2           | 2                    | 4           | 4          | 172\.30\.80\.4   | 172\.20\.80\.4   |                                                                                |
| lclient\.winix\.lab   | Complete    | N/A         | 11/4/2020            | 11/11/2020         | All                       | CentOS 7            | Linux Client                                                  | 2           | 2                    | 4           | 4          | 172\.30\.80\.253 | 172\.20\.80\.253 | Currently connected to DNS, SMB, NTP                                           |
| wclient\.winix\.lab   | Complete    | N/A         | 11/6/2020            | 11/13/2020         | All                       | Windows Server 2016 | Windows Client                                                | 2           | 2                    | 4           | 6          | 172\.30\.80\.80  | 172\.20\.80\.80  |                                                                                |
| ntp\.winix\.lab       | Complete    | Complete    | 11/8/2020            | 11/15/2020         | Brendan                   | CentOS 7            | NTP Server \(Value Added \#1\)                                | 1           | 2                    | 2           | 2          | 172\.30\.80\.5   | 172\.20\.80\.5   | Currently serving lclient                                                      |
| ntp2\.winix\.lab      | Complete    | Complete    | 11/8/2020            | 11/15/2020         | Brendan                   | CentOS 7            | Secondary NTP Server                                          | 1           | 2                    | 2           | 2          | 172\.30\.80\.8   | 172\.20\.80\.8   |                                                                                |
| dc01\.winix\.lab      | Complete    | Complete    | 10/19/2020           | 11/15/2020         | Kolawole                  | Windows Server 2019 | Master Windows Admin Center \(WAC\) & AD Domain Controller    | 2           | 2                    | 4           | 8          | 172\.30\.80\.6   | 172\.20\.80\.6   |                                                                                |
| dc02\.winix\.lab      | Complete    | Complete    | 10/19/2020           | 11/15/2020         | Kolawole                  | Windows Server 2019 | Secondary Windows Admin Center \(WAC\) & AD Domain Controller | 2           | 2                    | 4           | 8          | 172\.30\.80\.7   | 172\.20\.80\.7   | Slave DNS server                                                               |
| target\.winix\.lab    | Complete    | Complete    | 10/30/2020           | 11/20/2020         | Kolawole                  | Windows Server 2019 | iSCSI Target                                                  | 2           | 2                    | 4         | 6           |                  | 172\.20\.80\.11  |                                                                                |
| init\.winix\.lab      | Complete    | Complete    | 11/2/2020            | 11/20/2020         | Kolawole                  | Windows Server 2019 | iSCSI Initiator                                               | 1            |  2                    | 2           | 8           |                  | 172\.20\.80\.12  |                                                                                |
| backup\.winix\.lab    | Complete    | Complete    | 11/5/2020            | 11/30/2020         | Nicholas                  | windows Server 2019 | Veeam Backup                                                  | 2           | 2                    | 4           | 6          |                  | 172\.20\.80\.13  | researched/not configured                                                      |
| inventory\.winix\.lab | Complete    | Complete    | 11/5/2020            | 11/30/2020         | Nicholas                  | windows Server 2016 | Spiceworks/Inventory                                          | 2           | 2                    | 4           | 6          |                  | 172\.20\.80\.14  | researched/not configured                                                      |
| sql\.winix\.lab       | In Progress | Not Started | 11/11/2020           | 11/30/2020         | Nicholas                  | Ubuntu 18           | Azure SQL                                                     | 0           | 0                    | 0           | 0          |                  | 172\.20\.80\.37  | public ip 13\.90\.78\.8 \(for ssh connection\)                                 |
| azure webserver       | Complete    | Complete    | 11/11/2020           | 11/30/2020         | Nicholas                  | Ubuntu 18           | Azure Web Server                                              | 0           | 0                    | 0           | 0          |                  | 172\.20\.80\.36  | public ip 104\.211\.0\.91 \(for ssh connection\)    
| wac\.winix\.lab       | Complete    | In Progress | 11/11/2020           | 11/30/2020         | Kolawola                  | Windows Server 2019 | Windows Admin Center                                          | 4            | 2                     |8           | 6           |                  | 172\.20\.80\.15   |

#### Appendix 2) Testing Plan Sample

| Assigned to | Pass or Fail | Date Test Completed | Source                | Destination                               | Ping Successful | Service    | Test Parameters                                                                                                       | Expected Result                                       | Actual Result | Image File / Proof          |
|-------------|--------------|---------------------|-----------------------|-------------------------------------------|-----------------|------------|-----------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|---------------|-----------------------------|
| Brendan     | Pass         | 11/9/2020           | ntp\.winix\.lab       | N/A                                       | N/A             | NTP        | enter “systemctl status ntpd” in terminal to check status of service                                                  | Running, enabled, synced with servers                 | As expected   | CST8248 NTP %1\.PNG         |
| Brendan     | Pass         | 11/9/2020           | lclient\.winix\.lab   | ntp\.winix\.lab                           | Yes             | NTP        | run “ntpq \-p” command to check sync status; run “timedatectl” to verify “NTP enabled” is set to “yes”                | Running, enabled, synced with server                  | As expected   | CST8248 NTP %2\.PNG         |
| Brendan     | Pass         | 11/9/2020           | wclient\.winix\.lab   | ntp\.winix\.lab                           | Yes             | NTP        | check advanced settings for “Date and Time” and check for NTP sync details and status                                 | Running and synced with server                        | As expected   | CST8248 NTP %3\.PNG         |
| Brendan     | Pass         | 11/9/2020           | fileserve\.winix\.lab | wclient\.winix\.lab & lclient\.winix\.lab | Yes             | SMB        | run smbstatus command to verify that both clients are connected; run systemctl to verify service is running on server | Service running and both clients connected to server  | As expected   | CST8248 SMB %1\.PNG         |
| Brendan     | Pass         | 11/9/2020           | lclient\.winix\.lab   | fileserve\.winix\.lab                     | Yes             | SMB        | run two commands to connect to server and list directories; next command to list shares on server                     | Can connect to server and list directories and shares | As expected   | CST8248 SMB %2\.PNG         |
| Brendan     | Pass         | 11/9/2020           | wclient\.winix\.lab   | fileserve\.winix\.lab                     | Yes             | SMB        | connect to file server and show share in File Explorer window                                                         | Can connect and list contents of share directory      | As expected   | CST8248 SMB %3\.PNG         |
| Brendan     | Pass         | 11/9/2020           | webserve\.winix\.lab  | N/A                                       | N/A             | HTTP/HTTPS | run systemctl to verify service running and netstat command to check status of ports 443 and 80                       | Running, ports 80, 443                                | As expected   | CST8248 HTTP\-HTTPs %1\.PNG |
| Brendan     | Pass         | 11/9/2020           | lclient\.winix\.lab   | webserve\.winix\.lab                      | Yes             | HTTP/HTTPS | enter http://www\.winix\.lab in Firefox browser                                                                       | Webpage should open to default page                   | As expected   | CST8248 HTTP\-HTTPs %2\.PNG |
| Brendan     | Pass         | 11/9/2020           | lclient\.winix\.lab   | webserve\.winix\.lab                      | Yes             | HTTP/HTTPS | enter https://secure\.winix\.lab in Firefox browser, access certificate information                                   | Webpage should open; cert should be accessible        | As expected   | CST8248 HTTP\-HTTPs %3\.PNG |
| Brendan     | Pass         | 11/9/2020           | webserve\.winix\.lab  | N/A                                       | Yes             | HTTP/HTTPS | use two netstat commands to list all IPs connected to ports 80 and 443                                                | Both clients should appear as connected to both ports | As expected   | CST8248 HTTP\-HTTPs %4\.PNG |
| Kolawole    | Pass         | 11/12/2020          | dc1\.winix\.lab       | N/A                                       | Yes             | AD/DNS     | nslookup                                                                                                              |                                                       |               |                             |
| Kolawole    | Pass         | 11/12/2020          | dc2\.winix\.lab       | N/A                                       | Yes             | AD/DNS     |                                                                                                                       |                                                       |               |                             |
| Nicholas    | Pass         | 11/13/2020          | backup\.winix\.lab    | dc1\.winix\.lab                           | yes             | DNS        | Join Domain winix\.lab                                                                                                | Join Successfully                                      | As expected   |                             |
| Nicholas    | Pass         | 11/25/2020          | backup\.winix\.lab    | webserver\.winix\.lab                     | yes             | Backup     | Backup http server                                                                                                    | Backup Successfully                                    | As expexted  |                             |
| Nicholas    | Pass         | 11/25/2020          | inventory\.winix\.lab | winix\.lab                                | yes             | Inventory  | Gather info of the systems on winix.lab and log them                                                            | Logged winix.lab                                       | As expected  |                             |
| Nicholas    | Pass         | 11/25/2020          | Azure Web Server       | N/A                                      | yes             | Http       | Access Webserver address 104.211.0.91 from anywhere                                                                   | Access website                                         | As expected  |                             |
| Nicholas    | Fail         | 11/25/2020          | Azure SQL Server       | N/A                                       | yes             | PostgreSQL | Start the database                                                                                                    | Database starts                                        | Postmaster does not exist  |                             |

#### Appendix 3) Sample Firewall Configuration

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


#### Appendix 4) Network Topology

![Table](https://github.com/hanc0035/Winix/blob/master/images/UpdatedWinixTopology.png)

### References 

[1]I. Spiceworks, "SpiceWorks Install onto a VM with vSphere Beginning to End", Community.spiceworks.com, 2015. [Online]. Available: https://community.spiceworks.com/how_to/123190-spiceworks-install-onto-a-vm-with-vsphere-beginning-to-end. [Accessed: 10- Dec- 2020].

[2]"Step 1. Start Setup Wizard - Veeam Backup Guide for Hyper-V", Veeam Software Help Center, 2015. [Online]. Available: https://helpcenter.veeam.com/docs/backup/hyperv/install_vbr_launch.html?ver=100. [Accessed: 10- Dec- 2020].

[3]"Manage resource groups - Azure portal - Azure Resource Manager", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups. [Accessed: 10- Dec- 2020].

[4]"Create a virtual network - quickstart - Azure portal - Azure Virtual Network", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/virtual-network/quick-create-portal. [Accessed: 10- Dec- 2020].

[5]"Add, change, or delete an Azure virtual network subnet", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-subnet. [Accessed: 10- Dec- 2020].

[6]"Quickstart - Create a Linux VM in the Azure portal - Azure Virtual Machines", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal. [Accessed: 10- Dec- 2020].

[7]"Quickstart - Create a Linux VM in the Azure portal - Azure Virtual Machines", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal. [Accessed: 10- Dec- 2020].

[8]"Set up PostgreSQL on a Linux VM - Azure Virtual Machines", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/postgresql-install. [Accessed: 10- Dec- 2020].

[9]"Create, change, or delete an Azure network security group", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group. [Accessed: 10- Dec- 2020].

[10]"PostgreSQL and NodeJS", Mherman.org. [Online]. Available: https://mherman.org/blog/postgresql-and-nodejs/. [Accessed: 10- Dec- 2020].

[11]"What is Windows Admin Center", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/understand/what-is. [Accessed: 10- Dec- 2020].

[12]C. Taylor, "What Is iSCSI and How Does It Work?", Enterprisestorageforum.com. [Online]. Available: https://www.enterprisestorageforum.com/storage-hardware/iscsi.html. [Accessed: 10- Dec- 2020].

[13]"Install Active Directory Domain Services (Level 100)", Docs.microsoft.com. [Online]. Available: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-#BKMK_GUI. [Accessed: 10- Dec- 2020].
