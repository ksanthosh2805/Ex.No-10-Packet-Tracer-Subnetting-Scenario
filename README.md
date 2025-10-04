# Ex. No: 10 – Packet Tracer: Subnetting Scenario
## Date: 03-10-2025
## Name: Santhosh K
## RegNo: 212223100050
<br>

## Objective
Design, configure, and verify an IPv4 subnetting scheme in Cisco Packet Tracer.<br>
•	Subnet the 192.168.100.0/24 network into multiple subnets.<br>
•	Assign subnets to LANs and WAN link.<br>
•	Configure IP addressing on routers, switches, and PCs.<br>
•	Verify connectivity using ping and router show commands.<br>
<br>
________________________________________

## Apparatus / Tools Required
•	Cisco Packet Tracer<br>
•	2 Routers (R1, R2 – 2911 or equivalent)<br>
•	4 Switches (S1, S2, S3, S4)<br>
•	4 PCs (PC1–PC4)<br>
•	Copper straight-through cables for LAN links<br>
•	Serial DCE/DTE cable for WAN link<br>
________________________________________

## Network Topology Diagram

<img width="648" height="342" alt="image" src="https://github.com/user-attachments/assets/d0ba430e-1f90-4201-817f-af3b4e3f0ce0" />

________________________________________

## Subnetting Table

| Subnet     | Network ID   | First Usable Address | Last Usable Address | Broadcast Address | Prefix | Subnet Mask     | 
|------------|--------------|----------------------|---------------------|-------------------|--------|-----------------|
| **Subnet1**  | 192.168.0.0  | 192.168.0.1          | 192.168.0.30       | 192.168.0.31     | /27    |255.255.255.224  |
| **Subnet2**  | 192.168.0.32 | 192.168.0.33         | 192.168.0.62       | 192.168.0.63     | /27    |255.255.255.224  |
| **Subnet3**  | 192.168.0.64 | 192.168.0.65         | 192.168.0.94       | 192.168.0.95     | /27    |255.255.255.224  |
| **Subnet4**  | 192.168.0.96 | 192.168.0.97         | 192.168.0.126       | 192.168.0.127     | /27    |255.255.255.224  |
| **Subnet5(WAN)**  | 192.168.0.128 | 192.168.0.129         | 192.168.0.130       | 192.168.0.131     | /30    |255.255.255.252  |

## Addressing Table

| Device |	Interface |	IP Address               |	Subnet Mask |	Default Gateway |
|--------|------------|--------------------------|--------------|-----------------|
| R1     |	G0/0	    | 192.168.100.1     |	255.255.255.224	| N/A             |
| R1     |	G0/1      |	192.168.100.33	  | 255.255.255.224	| N/A             |
| R1     |	S0/0/0    | 192.168.100.129 |	255.255.255.252 | N/A             |
| R2     |	G0/0      | 192.168.100.65 | 255.255.255.224 |	N/A             |
| R2     |	G0/1      |	192.168.100.97 | 255.255.255.224 |	N/A             |
| R2     |	S0/0/0    |	192.168.100.130 |	255.255.255.252 |	N/A             |
| S1     |	VLAN1     |	192.168.100.2 |	255.255.255.224 |	R1 G0/0         |
| S2     |	VLAN1     |	192.168.100.34 | 255.255.255.224 |	R1 G0/1         |
| S3     |	VLAN1     |	192.168.100.66 | 255.255.255.224 |	R2 G0/0         |
| S4     |	VLAN1     |	192.168.100.98 | 255.255.255.224 |	R2 G0/1         |
| PC1    | 	NIC       |	192.168.100.30  |	255.255.255.224 |	R1 G0/0         |
| PC2    |	NIC       |	192.168.100.62 | 255.255.255.224 |	R1 G0/1         |
| PC3    |	NIC       |	192.168.100.94 | 255.255.255.224 |	R2 G0/0         |
| PC4    |	NIC       |	192.168.100.126 |	255.255.255.224 |	R2 G0/1         |

<br>

## Procedure
### Part 1: Subnet the Assigned Network
1.	Start with network: 192.168.100.0/24.<br>
2.	Requirements: Each LAN ≥ 25 hosts.<br>
3.	Choose subnet mask: /27 (255.255.255.224) → 32 hosts per subnet.<br>
4.	Subnets:<br>
o	Subnet 0 → LAN (R1 G0/0)<br>
o	Subnet 1 → LAN (R1 G0/1)<br>
o	Subnet 2 → LAN (R2 G0/0)<br>
o	Subnet 3 → LAN (R2 G0/1)<br>
o	Subnet 4 → WAN (R1–R2 Serial link)<br>

### Part 2: Configure the Devices
#### Router R1<br>
enable<br>
configure terminal<br>
hostname R1<br>
!<br>
interface g0/0<br>
 ip address <Subnet0-1stHost> 255.255.255.224<br>
 no shutdown<br>
!<br>
interface g0/1<br>
 ip address <Subnet1-1stHost> 255.255.255.224<br>
 no shutdown<br>
!<br>
interface s0/0/0<br>
 ip address <Subnet4-1stHost> 255.255.255.252<br>
 clock rate 64000<br>
 no shutdown<br>
end<br>
copy running-config startup-config<br>

#### Switches (S1–S4)<br>
interface vlan1<br>
 ip address <2ndHostOfSubnet><br>
 no shutdown<br>
ip default-gateway <RouterInterface><br>
#### PCs (PC1–PC4)<br>
•	NIC IP → Last host of each subnet.<br>
•	Subnet mask → 255.255.255.224 (LANs), 255.255.255.252 (WAN).<br>
•	Default gateway → Router interface of subnet.<br>
<br>
### Part 3: Verification & Testing
•	On Routers:<br>
o	show ip interface brief<br>
o	show ip route<br>
•	On PCs:<br>
o	Ping default gateway<br>
o	Ping across LANs (PC1 ↔ PC3)<br>
o	Ping across WAN (PC2 ↔ PC4)<br>
<br>
### Commands Used (Summary)
•	Mode/navigation: enable, configure terminal, end<br>
•	Interface config: interface g0/0, ip address, no shutdown<br>
•	Show/verify: show ip interface brief, show ip route<br>
•	Save: copy running-config startup-config<br>
<br>
________________________________________

## Output (Attach Screenshots)
### •	show ip interface brief on R1<br>

<img width="735" height="144" alt="Screenshot 2025-10-03 094921" src="https://github.com/user-attachments/assets/87ce4ea1-e77c-4c9f-8a42-f1687252cc53" />

### •	Successful pings<br>

#### Ping from R1 -> PC1
<img width="695" height="125" alt="Screenshot 2025-10-03 095003" src="https://github.com/user-attachments/assets/e96d24bb-56dc-4d00-9d1c-ed241a4f2bff" />

#### Ping from R1 -> PC2
<img width="657" height="129" alt="Screenshot 2025-10-03 095032" src="https://github.com/user-attachments/assets/b34fb66a-268a-4829-818b-9ac041f08d7b" />

#### Pirng from S3 to PC3
<img width="659" height="134" alt="Screenshot 2025-10-03 095128" src="https://github.com/user-attachments/assets/e97fe877-e054-4c76-932b-325b00207530" />

#### Ping from S3 to R3
<img width="677" height="133" alt="Screenshot 2025-10-03 095200" src="https://github.com/user-attachments/assets/e41c75cb-5acf-43c8-b490-728c80100cf1" />

#### Ping from PC4 to R4
<img width="532" height="250" alt="Screenshot 2025-10-03 095254" src="https://github.com/user-attachments/assets/985e6bcc-fa98-4a40-a5a8-247f77d4112a" />

<br>

## Result
The IPv4 subnetting scheme was successfully designed and implemented. Routers, switches, and PCs were configured with correct addressing. Connectivity within LANs and across WAN was verified.
