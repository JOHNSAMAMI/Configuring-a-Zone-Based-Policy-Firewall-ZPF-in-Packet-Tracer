# Configuring-a-Zone-Based-Policy-Firewall-ZPF-in-Packet-Tracer

Project Overview
This project focused on configuring a Zone-Based Policy Firewall (ZPF) on a Cisco router to manage traffic between internal and external networks. The objective was to secure internal resources while allowing appropriate access to external services.
Business Understanding
Implementing a ZPF is essential for modern network security. It enables organizations to define policies that control the flow of traffic based on zones rather than traditional access lists. This enhances security by allowing only desired traffic and blocking unauthorized access.
Tools and Frameworks Used
•	Packet Tracer: A network simulation tool for configuring Cisco devices.
•	Cisco IOS Commands: Used for router configuration.
Project Description
The project was executed in several stages, from verifying network connectivity to configuring and testing the ZPF. Below is a detailed breakdown of the tasks performed:
Topology and Addressing Table:
•	Configured three routers (R1, R2, R3) and several PCs, ensuring proper IP addressing and connectivity.
Objectives and Configuration Steps:
Step 1: Verify Basic Network Connectivity
•	Before configuring the ZPF, verified connectivity by pinging between internal and external devices.
•	Tests:
o	From PC-A, ping PC-C (192.168.3.3) to ensure connectivity.
o	SSH access to R2 from PC-C using Admin credentials.
o	Open a web browser on PC-C and access PC-A (192.168.1.3) to display the welcome page.
Step 2: Create the Firewall Zones on R3
1.	Enable Security Technology Package:
o	Checked the current technology package and enabled the Security package if it wasn't active.
R3(config)# license boot module c1900 technology-package securityk9
o	Accepted the EULA and saved the configuration.
2.	Create Internal and External Zones:
•	Created an internal zone named IN-ZONE and an external zone named OUT-ZONE.
R3(config)# zone security IN-ZONE
R3(config-sec-zone)# exit
R3(config)# zone security OUT-ZONE
Step 3: Identify Traffic Using a Class-Map
1.	Create an ACL for Internal Traffic:
o	Defined an ACL (101) to permit all IP traffic from the 192.168.3.0/24 network.
R3(config)# access-list 101 permit ip 192.168.3.0 0.0.0.255 any
2.	Create a Class Map:
•	Created a class map (IN-NET-CLASS-MAP) to match the defined ACL.
R3(config)# class-map type inspect match-all IN-NET-CLASS-MAP
R3(config-cmap)# match access-group 101
Step 4: Specify Firewall Policies
1.	Create a Policy Map:
o	Defined a policy map (IN-2-OUT-PMAP) to inspect matched traffic.
R3(config)# policy-map type inspect IN-2-OUT-PMAP
R3(config-pmap)# class type inspect IN-NET-CLASS-MAP
R3(config-pmap-c)# inspect
Step 5: Apply Firewall Policies
1.	Create Zone-Pair:
o	Established a zone-pair (IN-2-OUT-ZPAIR) to link the internal and external zones.
R3(config)# zone-pair security IN-2-OUT-ZPAIR source IN-ZONE destination OUT-ZONE
R3(config-sec-zone-pair)# service-policy type inspect IN-2-OUT-PMAP
2.	Assign Interfaces to Zones:
•	Assigned the appropriate interfaces to the IN-ZONE and OUT-ZONE.
R3(config)# interface g0/1
R3(config-if)# zone-member security IN-ZONE
R3(config-if)# exit
R3(config)# interface s0/0/1
R3(config-if)# zone-member security OUT-ZONE
3.	Save Configuration:
o	Saved the running configuration to ensure all changes persist after a reboot.
Step 6: Test Firewall Functionality
1.	From IN-ZONE to OUT-ZONE:
o	Ping Test: From PC-C, ping PC-A (192.168.1.3) to verify access.
o	SSH Test: SSH into R2 from PC-C to ensure session establishment.
2.	Monitor Established Sessions:
o	Used show policy-map type inspect zone-pair sessions to check the established sessions between the zones.
3.	From OUT-ZONE to IN-ZONE:
o	Ping Test: Attempted to ping PC-C from PC-A and R2, expecting failures, confirming the firewall's effectiveness.
Conclusion: The successful configuration of a Zone-Based Policy Firewall on R3 demonstrated its capability to secure internal resources while allowing controlled access to external resources. The tests confirmed that the firewall effectively blocked unauthorized access while permitting legitimate traffic.



