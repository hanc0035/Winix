
 

Team Winix Technical Report 

Team Submission 

Term Project CST8248 

Submitted 2020-12-03 

 

 

Team Members: 

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

 
Executive Summary 

 

Executive Summary will include the following: 

Short summary 

Should detail the key points for decision making purposes. 

Consider the purpose of your document while writing the executive summary 

 

 

 

 

Introduction: 

Intro will include the following: 

Introduce the document 

Describe the purpose of the document 

Describe the main content 

Scope of the project 

 

 

 

 

Background 

 

This document is intended to be read by the Computer System Technicians (CSTs) working for our client, Winix Laboratories Inc. The document will describe configuration decisions and other details that are intended to assist the CSTs with maintaining the services.  

For most of the services, individual servers are configured to perform specific tasks, such as the SMB file server on machine fileserve.winix.lab. Service stacking is limiting to the Spiceworks, DNS and Veeam services for reasons described later in this document. The use of individual systems tasked with one service is intended to minimize the complexity of server maintenance and facilitate easier backups and restores. This “modular” service design also aids in the creation of additional secondary or tertiary services as needed. Some of the critical services, such as ADDS, DNS and NTP already have secondary servers that can be used in the event of a failure on the primary systems.  

 

The Background will include: 

What does the reader need to understand before exploring this document further 

Who is the intended reader 

 

 

 

Discussion 

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

 

 
 

 

 

Conclusion 

 

The Conclusion will include: 

What was learned. 

Summarize document 

DO NOT introduce new content here!!! 

This is the end of the content/writing section. 

 

 

 

 

 

Appendices 

 

The Appendices can include the following: 

Stuff that does not fit in the body of the document 

Config files go well here 

Unresolved troubleshooting  

We probably won’t have much info to include here, so we should copy/paste our config files here so that we have enough content. We will also include our test plan in the appendices. 

 

 

 

 

References 

 

Please copy/paste the links to any sites/sources you used. Brendan can convert the links and ISBNs into formatted IEEE references. 

 
