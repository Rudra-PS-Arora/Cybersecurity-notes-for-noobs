**Introduction to SIEM(👉 SIEM is a concept, not a single tool.)**

Security Information and Event Management system (SIEM) is the core security solution that a SOC analyst uses in the security operations center.

Security solution means the tools, techniques and people required to tackle an incident or defend a system in general.

---------------------------Logs Everywhere, Answers Nowhere-------------------------

Multiple devices in a network communicate with each other and, most of the time, with the Internet through a router.

These devices continuously generate logs of the activities that occur within them. We can also call these devices log sources.

The logs they generate serve as a trail of all the activities and are extremely helpful for identifying malicious activities or general troubleshooting.


 		TYPES OF LOG SOURCE

1) Host-Centric Log Sources (in short logs for OS )

These log sources capture events that occurred within or related to the host. Devices that generate host-centric logs include Windows, Linux, servers, etc. Some examples of host-centric logs are:

A user accessing a file
A user attempting to authenticate.

2) Network-Centric Log Sources

Network-related logs are generated when the hosts communicate with each other or access the internet to visit a website. Devices that generate network-centric logs are firewalls, IDS/IPS, routers, etc. Some examples of network-centric logs are:

SSH connection
A file being accessed via FTP

Web traffic
A user accessing the company's resources through VPN.
Network file sharing Activity

**INSHORT (REVISION) --> log sources generate logs, we analyze them, and identify malicious activities.**


Cons of logs sources in analysing them**


1.Numerous Log Sources A network has many log sources, which generate hundreds of events per second. These logs are scattered across different devices, and examining the logs on each device one by one in case of an incident can be tedious.**

2.No Centralization As logs reside on the machines on which they are generated,  you may need to connect with each log source via SSH, RDP, etc., to analyze logs from multiple log sources. This is very inefficient and can waste a lot of your valuable time during the investigations.**

3.Limited Context Individual logs cannot tell the whole story of an activity. During any incident, the individual activities on different log sources may seem harmless. But if these logs are correlated, they can indicate a whole different story. For instance, you observed a file access event in a system, which is generally normal activity. However, if you correlate different log sources, you might come to know that this file was accessed by a user who accessed this machine through lateral movement after compromising another machine in the network.**

4.Limited Analysis The log sources generate numerous logs per second, and analyzing all the logs from all the devices manually to identify any abnormal activity is nearly impossible for humans. Realistically, the analysts will miss a lot of important logs in between the analyses due to their huge number.**

5. Format Issues Different log sources generate logs in various formats. Analysts need to know all these formats to analyze them, which can be extremely difficult, especially when dealing with numerous log sources in a network.**

-------------------------WHY SIEM-----------------------------------

Security Information and Event Management (SIEM) is a security solution that collects logs from various types of log sources, standardizes their format into a consistent one, correlates them, and detects malicious activities using detection rules.


IMPORTANT-->

Endpoints are devices where users or programs directly interact with a network.**
Laptops, Desktops, Workstations, servers etc**

A network is a collection of interconnected systems, devices, and services within an organization that communicate with each other to share data and resources.**



Corporate Network**
├── Endpoints (users)**
├── Servers (services)**
├── Network Devices (traffic control)**
└── Security Tools (visibility)**

An attacker moves from one compromised system to another different system inside the same network.**
The attacker moves between systems of the same privilege level.**
Vertical movement: increasing privileges on a system**

Features of SIEM

1. Centralized Log Collection

SIEM collects logs from all sources (endpoints, servers, firewalls, etc.) and centralizes them in one place.

2. Normalization of Logs

Raw logs are of different formats and sizes. A Windows log does not look the same as a Linux log. Since a SIEM solution centralizes these logs in one place, it also ensures that all the logs are broken down into different fields and presented in one consistent format. Breaking down a log into several fields for ease of understanding is known as Parsing, and converting all the logs of various log sources into one consistent format is known as Normalization. 

3.Correlation of Logs

Individual logs are not very useful. SIEM correlates the logs of different sources and finds any relationship between them. This helps to identify malicious activity by analyzing its pattern. For instance, let's take a look at the following activities happening in a system during a 5-minute timeframe. 

Haris logs in via VPN from an IP that he never has previously used
Haris accesses some documents on a shared drive
Haris executed a PowerShell script
The system makes an outbound network connection

4.Real-time Alerting
5.Dashboards and Reporting

[Splunk is a platform for collecting, storing, and analysing machine data. It provides various tools for analysing data, including search, correlation, and visualisation. It is a powerful tool that organisations of all sizes can use to improve their IT operations and security posture.]

There are different SIEM products,
But they differ by:
Architecture (on-prem / cloud / hybrid)
Ease of use
Detection style
Cost \& scale
eg - Splunk

| SIEM         | Best for                  |

| ------------ | ------------------------- |

| Splunk ES    | Large orgs, deep analysis |

| Elastic SIEM | Learning, budget setups   |

| Sentinel     | Cloud / Azure             |

| QRadar       | Rule-based detection      |

| ArcSight     | Legacy enterprises        |



Splunk is not only a SIEM — it’s a data analytics platform.

👉 When used for security, it becomes:
Splunk Enterprise Security (Splunk ES)\\
Analysts search using SPL (Search Processing Language)
more examples:
Elastic SIEM (ELK Stack)
IBM QRadar
Microsoft Sentinel
ArcSight

----------------------------Log Sources and Ingestion------------------------------

Ingestion means collecting data from various sources and storing them for analysis or processing.
Let's see some log of some common machines:

(a) Windows Machine

1. see logs By Event viewer (search in window's search bar)
2. Each log is assigned an unique event ID 



(b) Linux Machine

Linux OS stores all the related logs, such as events, errors, warnings, etc. These are then ingested into SIEM for continuous monitoring. Some of the common locations where Linux stores logs are:

/var/log/httpd		: Contains HTTP Request  / Response and error logs.

/var/log/cron	: Events related to cron jobs are stored in this location.

/var/log/auth.log and /var/log/secure	: Stores authentication-related logs.

/var/log/kern	: This file stores kernel-related events.


(c) Web Server

It is important to monitor all requests/responses coming in and out of the web server for any potential web attack attempt. In Linux, common locations to write all apache-related logs are /var/log/apache or /var/log/httpd.


Log Ingestion--->

(1) Agent / Forwarder

These SIEM solutions provide a lightweight tool called an agent (forwarder by Splunk) that gets installed on the Endpoint. It is configured to capture and send all the important logs to the SIEM server.

(2) Syslog

Syslog is a widely used protocol to collect data from various systems like web servers, databases, etc., and send real-time data to the centralized destination.

(3) Manual Upload

Some SIEM solutions, like Splunk, ELK, etc., allow users to ingest offline data for quick analysis. Once the data is ingested, it is normalized and made available for analysis.

(4) Port-Forwarding

SIEM solutions can also be configured to listen on a certain port, and then the endpoints forward the data to the SIEM instance on the listening port.


------------------------Alerting Process and Analysis-------------------------------

Threat: Potential source of harm
Event: Logged activity
Alert: Suspicious pattern detected
Incident: Confirmed security breach

User or system performs an activity →
activity is written into logs →
logs create events →
suspicious events raise alerts →
confirmed malicious activity becomes a threat →
when that threat causes damage, it becomes an incident →
SOC team responds to the incident.



(1) Behind the Triggered Alerts are rules set on the SEIM software.

These rules play an important role in the timely detection of threats

Detection rules are pretty much logical expressions set to be triggered. A few examples of detection rules are:

(a) If a user gets five failed Login Attempts in 10 seconds, raise an alert for Multiple Failed Login Attempts

(b) If login is successful after multiple failed login attempts, raise an alert for Successful Login After multiple Login Attempts

(2)How is a detection rule created?

(a) A unique Event ID 104 is logged every time a user tries to remove or clear event
Hackers or pentester general remove logs so to tackle this a rule are be create like: 
Rule: If the Log source is WinEventLog AND EventID is 104 - Trigger an alert Event Log Cleared

(b)Adversaries use commands like whoami after the exploitation/privilege escalation phase. The following Fields will be helpful to include in the rule.

Log source: Identify the log source capturing the event logs
Event ID: Which Event ID is associated with Process Execution activity? In this case, Event ID 4688 will be helpful.
NewProcessName: Which process name will be helpful to include in the rule?



Rule: If Log Source is WinEventLog AND EventCode is 4688, and NewProcessName contains whoami, then Trigger an ALERT WHOAMI command Execution DETECTED

Alert Investigation

When monitoring SIEM, analysts spend most of their time on dashboards, as they display various key details about the network in a very summarized way. Once an alert is triggered, the events/flows associated with the alert are examined, and the rule is checked to see which conditions are met. Based on the investigation, the analyst determines if it's a True or False positive. Some of the actions that are performed after the analysis are:

Alert is a False Positive. It may require tuning the rule to avoid similar False positives from occurring again.
Alert is a True Positive. Perform further investigation.
Contact the asset owner to inquire about the activity.
Suspicious activity is confirmed. Isolate the infected host.
Block the suspicious IP.

----------------------------------------------------------------------------------

MITRE ATT&CK is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations.

TTP mapping means:
Taking observed activity (logs, alerts, incidents)
and matching it to the MITRE ATT&CK techniques that describe it.
In simple words:
“What we saw” → “Where it fits in MITRE ATT&CK”



