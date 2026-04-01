# SOC Workbooks and Lookups
## Discover useful corporate resources to help you structure and simplify L1 alert triage.

Task1: Intoduction
Alert triage is a complex process that often requires analysts to gather additional information about affected employees or servers. This room explores SOC workbooks designed to streamline alert triage and explains various lookup methods to quickly retrieve user and system context.

Learning Objectives
  Familiarise yourself with SOC investigation workbooks
  Learn where to find and how to use asset inventory in SOC
  Understand the importance of corporate network diagrams
  Practice workflow building inside an interactive interface
Prerequisites
  Complete the SOC L1 Alert Triage and Alert Reporting rooms
  Have practice with investigating common attack chains
  Understand the fundamental networking concepts
  Preferably, be familiar with the concept of SOAR playbooks
Answer: No answer required

Task2: Assets & Identities
Imagine having a night shift and looking into an alert saying that G.Baker logged into the HQ-FINFS-02 server. Then, the user downloaded the "Financial Report US 2024.xlsx" file from there and shared it with R.Lund. To correctly triage the alert and understand if the activity is expected, you will have to find answers to many questions:

  Who is G.Baker? What are their working hours and role in the company?
  What is the purpose and location of HQ-FINFS-02? Who can access it?
  Why could R.Lund need access to the corporate financial records?
Identity Inventory

Identity inventory is a catalogue of corporate employees (user accounts), services (machine accounts), and their details like privileges, contacts, and roles within the company. For the scenario above, identity inventory would help you get context about G.Baker and R.Lund, and make it simpler to decide if the activity was expected or not.

Example of Identities
 Sources of Identities
Optimizing tool selection...| Solution        | Examples                   | Description                                                                 |
|-----------------|----------------------------|-----------------------------------------------------------------------------|
| Active Directory | On-prem AD, Entra ID       | AD itself is an identity database, and it is commonly used by SOC.         |
| SSO Providers   | Okta, Google Workspace     | Cloud alternative for AD; an easy way to manage and search users.          |
| HR Systems      | BambooHR, SAP, HiBob       | Limited to employees only, but usually provides full employee data.        |
| Custom Solution | CSV or Excel Sheets        | It is common for IT or security teams to maintain their own solutions.     |

Asset Inventory
Asset inventory, also called asset lookup, is a list of all computing resources within an organisation's IT environment. Note that while "asset" is a vague term and can also refer to software, hardware, or employees, this room emphasises servers and workstations only. For the scenario above, asset inventory would help you get context about the HQ-FINFS-02 server.

## Looking at the identity inventory, what is the role of R.Lund at the company?
Answer: US Financial Adviser

## Checking the asset inventory, what data does the HQ-FINFS-02 server store?
Answer: Financial records

## Finally, does the file sharing from the scenario look legitimate and expected? (Yea/Nay)
Answer: Yea

Task3: Network Diagrams
Continuing identity and asset inventory topics, you might also need to look at the alert from a network point of view, especially in bigger companies. Consider the scenario where you are investigating a chain of related alerts based on firewall logs and want to give some meaning to the IPs you see:

08:00: An IP 103.61.240.174 is repeatedly connecting to a corporate firewall via port TCP/10443
08:23: Firewall logs show that the IP 103.61.240.174 was translated to an internal 10.10.0.53 IP
08:25: The IP 10.10.0.53 is scanning the 172.16.15.0/24 network range but does not find open ports
08:32: The same IP is now scanning the 172.16.23.0/24 network range, and the attack seems to be ongoing

Network Diagrams
To investigate the case above, you will have to find out what service is running at the 10443 port and why anyone would connect there. Then, identify the subnet the 10.10.0.53 IP belongs to and why it would ever try to connect to other subnets. A network diagram, a visual schema presenting existing locations, subnets, and their connections, is an answer to your questions:
Depending on a company's size and structure, you may see more complex diagrams, but their use for SOC analysts remains the same - to help understand suspicious network activity. In our scenario, you can refer to the network diagram and reconstruct the attack path as follows:
  Threat actor behind the 103.61.240.174 IP performed VPN brute force, targeting vpn.tryhatme.thm
  After a successful brute force and VPN login, the threat actor was assigned an IP from the VPN Subnet
  Then, the adversary tried to scan the Database Subnet, but was likely blocked by the firewall rules
  Seeing no success, the threat actor switches to the Office Subnet, looking for their next target

## According to the network diagram, which service is exposed on the TCP/10443 port?
Answer: VPN

## Now, which subnet would the server behind 172.16.15.99 IP belong to?
Answer: Database subnet

## Finally, does the scenario look like a True Positive (TP) or False Positive (FP)?
Answer: TP

Task4: Workbooks Theory
## Which SOC role would use workbooks the most (e.g. SOC Manager)?
Answer: SOC L1 Analyst

## What is the process of gathering user, host, or IP context using TI and lookups?
Answer: Enrichment

## Looking at the workbook example, what platform is used as an identity inventory source?
Answer: BambooHR

Task5: Workbooks Practice
## What flag did you receive after completing the first workbook?
Answer: THM{the_most_common_soc_workbook}

## What flag did you receive after completing the second workbook?
Answer: THM{be_vigilant_with_powershell}

## What flag did you receive after completing the third workbook?
Answer: THM{asset_inventory_is_essential}