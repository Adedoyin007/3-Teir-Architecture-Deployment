# 3-Teir-Architecture-Deployment
3-Teir-Architecture-Deployment

⸻

🚀 Azure 3-Tier Architecture Deployment (Secure Network + SQL Connectivity)

📌 Overview

This project demonstrates the deployment of a secure 3-tier architecture in Microsoft Azure, including:

* Virtual Network with segmented subnets
* Network Security Groups (NSGs) for controlled traffic flow
* Two Virtual Machines (Web + App tiers)
* Azure SQL Database (managed database service)
* End-to-end connectivity validation

The goal is to simulate a real-world multi-tier application environment with proper network isolation and security controls.

⸻

🏗️ Architecture Design

Components

Layer	Resource	Purpose
Web Tier	Web-VM	Handles incoming HTTP requests
App Tier	App-VM	Processes application logic
Data Tier	Azure SQL Database	Stores application data

⸻

🌐 Virtual Network Configuration

A Virtual Network was created in:

* Region: East US
* Address Space: 10.0.0.0/16

Subnets

Subnet Name	Address Range	Purpose
WebSubnet	10.0.1.0/24	Hosts Web-VM
AppSubnet	10.0.2.0/24	Hosts App-VM
DbSubnet	10.0.3.0/24	Reserved for database layer

⸻

🔐 Network Security Groups (NSGs)

NSGs were implemented to enforce strict traffic control between tiers.

WebSubnet NSG Rules

Priority	Port	Source	Action
100	80 (HTTP)	Internet	Allow
110	3389 (RDP)	My IP	Allow

⸻

AppSubnet NSG Rules

Priority	Port	Source	Action
100	80	WebSubnet (10.0.1.0/24)	Allow
110	3389	My IP	Allow

⸻

DbSubnet NSG Rules

Priority	Port	Source	Action
100	1433 (SQL)	AppSubnet (10.0.2.0/24)	Allow

⸻

🖥️ Virtual Machines Deployment

Web-VM

* Located in WebSubnet
* Configured with IIS to serve web content
* Public IP enabled for external access

App-VM

* Located in AppSubnet
* Acts as the middle-tier application server
* Used for testing database connectivity

⸻

🗄️ Azure SQL Database Deployment

Instead of deploying a database on a VM, a managed Azure SQL Database was created.

Key Details:

* Hosted on a Logical SQL Server
* Accessible via:

db-sql-server-unique.database.windows.net

Important Note:

Azure SQL Database is a PaaS service, meaning:

* No VM is required
* No direct private IP inside your subnet
* Access is controlled via firewall rules and endpoints

⸻

🔗 Database Connectivity Testing

Connectivity was tested from the App-VM using PowerShell.

Command Used:

Test-NetConnection db-sql-server-unique.database.windows.net -Port 1433

⸻

✅ Successful Output Summary:

Field	Value
Remote Address	20.x.x.x
Port	1433
Source Address	10.0.2.4
TcpTestSucceeded	True

⸻

🔍 Explanation

* DNS Resolution: Server name successfully resolved to a public IP
* Port 1433: Open and reachable
* Source Address: Confirms request originated from App-VM
* TcpTestSucceeded = True: Confirms successful connectivity

⸻

🧪 Additional Validation

An initial test failed due to DNS resolution issues, but a retry succeeded, confirming:

* Temporary DNS delay
* Correct configuration overall

⸻

🔒 Security Design Summary

* Direct internet access is restricted to Web tier only
* App tier only accepts traffic from Web tier
* Database only accepts traffic from App tier
* Principle of least privilege enforced across all layers

⸻

📈 Key Achievements

* Implemented secure 3-tier architecture
* Applied subnet-level traffic isolation
* Configured NSGs with granular rules
* Successfully validated database connectivity
* Used Azure PaaS services appropriately

⸻

⚠️ Limitations / Improvements

* Azure SQL Database is accessed via public endpoint
* Could be improved using:
    * Private Endpoint (for full isolation)
    * Azure Bastion (for secure VM access)
    * Application Gateway (for production-grade web layer)

⸻

🚀 Future Enhancements

* Implement Private Link for SQL Database
* Add Load Balancer / Application Gateway
* Automate deployment using ARM/Bicep/Terraform
* Integrate monitoring with Azure Monitor

⸻

🧾 Conclusion

This project successfully demonstrates how to design and deploy a secure, scalable, and layered cloud architecture in Azure.

It highlights:

* Proper network segmentation
* Controlled communication between tiers
* Secure database access
* Real-world cloud architecture principles

⸻
Below is a clear visual reprsentation of my deployed 3-teir archieture in azure
                  🌐 Internet
                         │
                         ▼
                ┌─────────────────┐
                │    Web-VM       │
                │ (IIS Enabled)   │
                │ 10.0.1.4        │
                └────────┬────────┘
                         │  HTTP (Port 80)
                         ▼
                ┌─────────────────┐
                │    App-VM       │
                │ Application Tier│
                │ 10.0.2.4        │
                └────────┬────────┘
                         │  SQL (Port 1433)
                         ▼
        ┌────────────────────────────────────┐
        │   Azure SQL Database (PaaS)        │
        │ db-sql-server-unique.database...   │
        │ Public Endpoint (20.x.x.x)         │
        └────────────────────────────────────┘


Network Security Flow visual 

Internet
   │
   ▼
[ NSG - WebSubnet ]
   ✔ Allow: HTTP (80) from Internet
   ✔ Allow: RDP (3389) from My IP
   │
   ▼
Web-VM
   │
   ▼
[ NSG - AppSubnet ]
   ✔ Allow: HTTP (80) from WebSubnet only
   ✔ Allow: RDP (3389) from My IP
   │
   ▼
App-VM
   │
   ▼
[ NSG - DB Rules ]
   ✔ Allow: SQL (1433) from AppSubnet only
   │
   ▼
Azure SQL Database

This architecture follows a secure 3-tier design pattern, where:

* The Web Tier is the only layer exposed to the internet
* The Application Tier is isolated and only accepts traffic from the Web tier
* The Database Tier is further restricted to accept traffic only from the Application tier

This ensures network segmentation, reduced attack surface, and enforcement of least privilege access.


📎 Author

Name: [Adedoyin]
Project: Azure 3-Tier Architecture Lab
