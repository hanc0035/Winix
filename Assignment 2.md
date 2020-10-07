# Assignment 2
## Project Quality and Testing
### Completed for CST8248 by Team Winix

#### Team members;
Brendan Marentette

Kolawole Adamo

Nicholas Hancin

### Quality Assurance:

  Each of the deliverables to be submitted must be thoroughly tested to ensure all aspects of the service function properly. This
includes ensuring the Test Plan is implemented properly and changed as needed to reflect ongoing changes to the various services.
Another expectation is that all Virtual Machines (VMs) are configured to support easy backups; backups can include snapshots taken
at set intervals or remote cloud backups to Veeam or a comparable service. All configuration files must be easily readable and 
consistent documentation is mandatory. Documentation will be an essential aspect of the service implementations since it will be 
required to ensure that ongoing maintanence is can be completed efficiently and with a minimal chance of errors or misconfigurations.

### Hardware Plan:

| Hostname           | CPU (# sockets; # cores) | RAM (GB) | Network Interfaces | Storage Capacity (GB) | Other              |
|--------------------|--------------------------|----------|--------------------|-----------------------|--------------------|
| ns1.winix.lab      | 2; 2                     | 4        | Red, Blue          | 20                    |                    |
| ns2.winix.lab      | 1; 2                     | 2        | Red, Blue          | 10                    |                    |
| fserver.winix.lab  | 1; 2                     | 3        | Red, Blue          | 20                    | Share Drive: 20 GB |
| webserve.winix.lab | 1; 2                     | 3        | Red, Blue          | 10                    |                    |
| lclient.winix.lab  | 1; 2                     | 2        | Red, Blue          | 15                    |                    |
| wclient.winix.lab  | 2; 2                     | 6        | Red, Blue          | 15                    |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |
|                    |                          |          |                    |                       |                    |

### Networking Plan:

Overview: Each of the VMs will have a Red and Blue interface. The Red interface will be used for all traffic within the network
and is intended to be enabled permanently. By comparison, the Blue interfaces are intended to only be active as needed to complete
updates and installation of content originating on remote web servers. As soon as the Blue interface is no longer needed, the best
practice will be to deactivate the interface.

| Hostname           | Service / Notes   | Red Network IP Address | Blue Network IP Address |
|--------------------|-------------------|------------------------|-------------------------|
| ns1.winix.lab      | Master DNS        | 172.30.80.1            | 172.20.80.1             |
| ns2.winix.lab      | Slave DNS         | 172.30.80.2            | 172.20.80.2             |
| fserver.winix.lab  | Samba Server      | 172.30.80.3            | 172.20.80.3             |
| webserve.winix.lab | HTTP/HTTPS Server | 172.30.80.4            | 172.20.80.4             |
| lclient.winix.lab  | Linux Client      | 172.30.80.253          | 172.20.80.253           |
| wclient.winix.lab  | Windows Client    | 172.30.80.254          | 172.20.80.254           |
|                    |                   |                        |                         |
|                    |                   |                        |                         |
|                    |                   |                        |                         |
|                    |                   |                        |                         |
|                    |                   |                        |                         |


### Test Plan:

Overview: Testing of the services and VMs will be conducted on an ongoing basis while the services are implemented. For example,
the DNS service running on ns1.winix.lab will be tested in stages to reflect the increasingly complexity of the service as forward
and reverse zones are created and a slave server is configured. It is required that the testing completed during implementation of 
a service should be documented for future reference and to aid with troubleshooting if/when issues arise.

| Source          | Destination             | Connectivity (icmp) | Service              | Test Command | Result | Comment                                                                                                 | Test Date |
|-----------------|-------------------------|---------------------|----------------------|--------------|--------|---------------------------------------------------------------------------------------------------------|-----------|
| Linux Client    | Linux DNS Server        |                     | DNS                  | dig          |        | Test name resolution on both master and slave servers; test reverse and forward zones                   |           |
| Linux Client    | Linux Samba Server      |                     | SMB                  |              |        | Test read and write abilities; test to ensure only valid user has access                                |           |
| Linux Client    | Linux HTTP/HTTPS Server |                     | HTTP/HTTPS           | nc           |        | Test secure site; ensure HTTPS is working and site is visible in browser; also test command line access |           |
|                 |                         |                     | Active Directory     |              |        |                                                                                                         |           |
|                 |                         |                     | Windows Admin Center |              |        |                                                                                                         |           |
| Windows Client  | Veeam Cloud             |                     | Veeam Backup         | N/A          |        | Test to ensure a backup can be successfully completed                                                   |           |
|                 |                         |                     | Spiceworks           |              |        |                                                                                                         |           |
| iSCSI initiator | iSCSI target            |                     | iSCSI                |              |        |                                                                                                         |           |
|                 |                         |                     | PostGreSQL           |              |        |                                                                                                         |           |
|                 |                         |                     |                      |              |        |                                                                                                         |           |
|                 |                         |                     |                      |              |        |                                                                                                         |           |
|                 |                         |                     |                      |              |        |                                                                                                         |           |
|                 |                         |                     |                      |              |        |                                                                                                         |           |
|                 |                         |                     |                      |              |        |                                                                                                         |           |
