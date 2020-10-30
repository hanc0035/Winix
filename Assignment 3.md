# Assignment 3
## Project Quality and Testing
### Completed for CST8248 by Team Winix

#### Team members;
Brendan Marentette

Kolawole Adamo

Nicholas Hancin


### 1) Which project elements are on track?

The DNS Servers and file server are on track. Both were completed according to their respective deadlines. The file server
deadline was set as October 23 and it was completed on October 22. The deadline for setting up the DNS Servers was set as
October 16. The initial configuration was completed on October 16 and extra troubleshooting was completed in the days 
that followed.


### 2) Which elements are ahead of schedule and by how much? To what have you reallocated this time?

Deploying the Linux Client VM was initially planned to be completed by November 15, however we're currently in the process
of completing its configuration. As of October 22, it has been configured as a client for the DNS, SMB and NTP services.
The Linux client will continue to be updated as new client services become available.
In addition, the Value Added #1 deliverable is approaching completion. This deliverable was originally scheduled to be completed
by November 15, but it is expected that it will be done by October 28.


### 3) Which elements are behind schedule and by how much?

As of the time of this assignment submission, no elements are overdue.


### 4) Identify the root cause of the delay. Is there a way to prevent these delays in the future.

As of the time of this assignment submission, no elements are overdue. If any future deliverables are at risk of falling behind
our schedule, we will discuss remediation during our soonest team meeting.


### 5) For each item behind schedule detail a strategy to get back on track.

As of the time of this assignment submission, no elements are overdue. At this point, we intend to complete the deliverables early if 
possible in order to give us as much time as possible to conduct testing and implement improvements as needed.


### 6) Update your project plan and include it with these reflections. Indicate where changes have been made.

Please refer to the Progress Log chart that has been included at the end of this document. It covers the scheduled start/end dates
in addition to system hardware and service information. 


### 7) Reflect upon your communications plan. 

Was it adequate thus far? Detail any changes you have made?
The communication plan has proven to be effective. Each of the weekly team meetings have been completed and the meeting minutes have
been posted in an accessible Microsoft OneDrive folder as planned (for future reference). So far the only change made to the proposed
schedule was to change the date of one of the meetings due to a timing conflict. This created no problems, since the meeting was 
rescheduled for the following day. As of today, there are no proposed changes to the communication plan and none are anticipated.


### 8) Examine your hardware plan. Is the hardware allocated to each virtual machine appropriate? Take this opportunity to right-size your virtual machines?

All hardware are seen to be allocated to virtual machines correctly. If any changes are needed in the future, they will be updated here.


### 9) Examine your network plan. Is your network plan adequate?

Some services have not been implemented yet, so they have been omitted from the topology diagram below. The network plan (as seen in the table at the bottom
of this document) has proven to be adequate. The only anticipated change is that we will likely phase out the Red Network as per the instructions Denis
outlined during his lecture on October 23, 2020.

![Topology](https://github.com/hanc0035/Winix/blob/master/Winix%20Topology.png)


### 10) Update your test plan for any completed services or features. Include your updated test plan in this document.

Most services are not completed but in progress while conducting the configuration test plan. Formal testing of each of the services will be completed and 
documented at a later date.


### 11) Are there any potential hurdles that your team can foresee at this point in the project?

Some of the remaining services we need to implement are paid application that we can use with a limited free trial. This means some services, including the Azure
Public Cloud deliverables in addition to Veeam backups will need to wait to be completed until a later date that is closer to the time that we demo. This represents
a minor potential challenge since we can't finish these services as early as many of the others.
Another potential challenge is that our team is unfamiliar with some services, namely the iSCSI Initiator and iSCSI Target. As a result, when we begin working on these 
services we will look into theory and practical materials to guide us in our implementation. In addition, we will consult with the Professors as needed when we have
questions.

### SUPPLEMENTARY: Progress Log including Updated Network and Hardware Plan

Note: Use the scroll bar below the table to view all columns

|      Hostname      |  Completion |   Testing   | Scheduled Start Date | Scheduled Due Date | Responsible Team Member/s |   Operating System  |                           Service                           | CPU Sockets | CPU Cores per Socket | Total Cores | RAM (GB) | Red Network IP | Blue Network IP |                 Notes                |
|:------------------:|:-----------:|:-----------:|:--------------------:|:------------------:|:-------------------------:|:-------------------:|:-----------------------------------------------------------:|:-----------:|:--------------------:|:-----------:|:--------:|:--------------:|:---------------:|:------------------------------------:|
| ns1.winix.lab      |   Complete  | In Progress |       10/9/2020      |     10/16/2020     | Brendan                   | CentOS 7            | Master DNS                                                  |      2      |           2          |      4      |     4    |   172.30.80.1  |   172.20.80.1   |                                      |
| ns2.winix.lab      |   Complete  | In Progress |       10/9/2020      |     10/16/2020     | Brendan                   | CentOS 7            | Slave DNS                                                   |      1      |           2          |      2      |     3    |   172.30.80.2  |   172.20.80.2   |                                      |
| fserver.winix.lab  |   Complete  | In Progress |      10/16/2020      |     10/23/2020     | Brendan                   | CentOS 7            | Samba Server                                                |      1      |           2          |      2      |     3    |   172.30.80.3  |   172.20.80.3   |                                      |
| webserve.winix.lab | In Progress | Not Started |      10/17/2020      |     10/24/2020     | Brendan                   |                     | HTTP/HTTPS Server                                           |             |                      |      0      |          |   172.30.80.4  |   172.20.80.4   |                                      |
| lclient.winix.lab  |   Complete  | In Progress |       11/4/2020      |     11/11/2020     | Brendan                   | CentOS 7            | Linux Client                                                |      2      |           2          |      4      |     4    |  172.30.80.253 |  172.20.80.253  | Currently connected to DNS, SMB, NTP |
| wclient.winix.lab  | In Progress | In Progress |       11/6/2020      |     11/13/2020     |                           | Windows Server 2016 | Windows Client                                              |      2      |           2          |      4      |     6    |  172.30.80.254 |  172.20.80.254  |                                      |
| ntp.winix.lab      |   Complete  | In Progress |       11/8/2020      |     11/15/2020     | Brendan                   | CentOS 7            | NTP Server (Value Added #1)                                 |      1      |           2          |      2      |     2    |   172.30.80.5  |   172.20.80.5   | Currently serving lclient            |
| dc1.winix.lab      |             | In Progress |      10/19/2020      |     10/26/2020     |                           | Windows Server 2019 | Master Windows Admin Center (WAC) & AD Domain Controller    |     |                      |      0      |          |   172.30.80.6  |   172.20.80.6   |                                      |
| dc2.winix.lab      |             | In Progess  |      10/19/2020      |     10/26/2020     |                           | Windows Server 2019 | Secondary Windows Admin Center (WAC) & AD Domain Controller |             |                      |      0      |          |   172.30.80.7  |   172.20.80.7|                                      |
|                    |             |             |      10/30/2020      |      11/6/2020     |                           |                     | iSCSI Target                                                |             |                      |      0      |          |                |                 |                                      |
|                    |             |             |       11/2/2020      |      11/9/2020     |                           |                     | iSCSI Initiator                                             |             |                      |      0      |          |                |                 |                                      |
|                    |             |             |      10/25/2020      |      11/1/2020     |                           |                     | Veeam Backup                                                |             |                      |      0      |          |                |                 |                                      |
|                    |             |             |      10/27/2020      |      11/3/2020     |                           |                     | Spiceworks/Inventory                                        |             |                      |      0      |          |                |                 |                                      |


