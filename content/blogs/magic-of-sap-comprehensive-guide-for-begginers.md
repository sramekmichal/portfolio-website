---
title: "The Magic of SAP: A Comprehensive Guide for Begginers"
date: 
draft: false
author: "Michal Šrámek"
devto: "https://dev.to/sramek5/the-magic-of-sap-a-comprehensive-guide-for-begginers-19jc"
tags:
  - SAP
  - ERP
  - Architecture
image: /images/blogs/sap-guide.jpg
description: "The Magic of SAP: A Comprehensive Guide for Begginers"
toc: true
---

In the process of our IT life, each of us will surely encounter the word SAP one day. But what is actually SAP and how does it actually work? The official vendor documentation is clear on this and defines SAP as following: `SAP is one of the world’s leading producers of software for the management of business processes.` That is not entirely clear. Even a literal German translation - **Systemanalyse Programmentwicklung** - will not help us too much.

SAP is basicly collection of Systems, Applications, and Products in Data Processing. It is provider of enterprise software solutions designed to enhance business processes and efficiency. One of SAP's key products is so-called SAP ECC (i.e. ERP Central Component), with extra indication R3, which refers to its architecture based on Real-Time data processing and a three-tier structure. 

This architecture includes the presentation layer, application layer, and database layer, working together to provide robust, scalable, and flexible Enterprise Resource Planning (ERP) solutions that support a wide range of business functions.

![Repository Creation](/images/blogs/sap-r3-architecture.png)

## Presentation Layer

The presentation layer in SAP R3 architecture is crucial for facilitating user interaction with the system. This layer provides input capabilities for users to manipulate the system and output functionalities for generating results based on user actions. The primary interface for users interacting with SAP applications is the SAP GUI, which is installed on individual machines. Through SAP GUI, users can access various SAP modules and execute transactions, run reports, and perform data entry tasks. The presentation layer ensures a seamless and intuitive user experience, translating complex backend processes into comprehensible visual elements, thereby enhancing user productivity and system usability.

## Application Layer

The application layer serves as the core of the system, processing business logic and managing communication between the presentation and database layers. A key component of this layer is the **Web Dispatcher**, which acts as a critical gateway between the Internet and SAP systems. It handles HTTP(s) requests, balances the load across multiple SAP NetWeaver application servers, and enhances system security by filtering URLs and supporting SSL encryption. The Web Dispatcher is compatible with both pure ABAP (high-level programming language cretaed by SAP) and Java systems.

Within the application layer, various **ABAP Processes** are essential for efficient system operations:

1. Dialog Process (DIA): Manages the interaction between the user and the system, handling immediate transaction requests.
2. Background Process (BGD): Executes long-running or scheduled tasks that do not require real-time user interaction.
3. Spool Process (SPO): Manages print requests and the overall printing process.
4. Update Process (UDP): Responsible for updating data in the database, ensuring that changes are correctly saved.
5. Enqueue Process (ENQ): Handles the locking of data to maintain consistency and integrity during concurrent access.

Additionally, the application layer includes crucial **ABAP Services**:

1. Message Service: Ensures load balancing across different SAP server instances, routing and delivering messages efficiently.
2. Enqueue Replication Server (ERS): Maintains a synchronized replica of the lock table on a secondary NetWeaver server, ensuring data consistency and enabling automatic failover in the event of a primary server failure.

Together, these components and services ensure that the application layer functions effectively, supporting robust and reliable enterprise resource planning for businesses.

## Database Layer

The database layer is responsible obviously for storing and retrieving data, forming the foundation of the entire system. While SAP does not provide its own database system, it supports integration with leading relational database management systems (RDBMS) such as Oracle, DB2, and HANA. HANA, a multi-model database, is particularly notable for its ability to store data in memory rather than on a disk, which significantly enhances processing speeds compared to traditional database management systems.

HANA's integration includes High Availability and Disaster Recovery (HA/DR) capabilities through so-called HANA System Replication (HSR). This mechanism ensures high availability by automatically switching to a standby host in case of a primary host failure. It uses _synchronous_ data replication to maintain data consistency between the primary and standby hosts, thus minimizing downtime and ensuring data integrity.

This layer's robustness is critical for the overall performance of the SAP system, as it handles large volumes of transaction data, supports complex queries, and ensures data integrity and security. The database layer's compatibility with top-tier relational database management system (RDBMS) and advanced features like HANA's in-memory storage and HSR replication make it a powerful component of the SAP ECC R3 architecture, enabling businesses to process and analyze data with exceptional speed and reliability.

## Trivia

The following abbreviations are also associated with SAP:
- PAS = Primary Application Server.
- AAS = Additional Application Server.
- ASCS = ABAP Central Services (core of the SAP application service)

The differences between PAS and AAS: 
- The PAS contains the ASCS, but an AAS does not. 
- In a system, there is only one PAS, but there can be multiple AASs. The number depends on the service requirements.

## Conclusion

SAP stands globally as a leading provider of enterprise software solutions, helping organizations streamline and optimize their business processes. With its robust architecture and integration capabilities, SAP enables efficient management of complex operations, enhancing productivity and decision-making. SAP's adaptability with various database systems and its advanced features ensure it remains a vital tool for businesses aiming for growth and operational excellence.