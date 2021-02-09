# :desktop_computer: Serverteknologi-HF2

Af Mark, Jacob & Mads

## Indhold:
* [Del-1](#del-1)
	* [VMWare](#vmware)
	* [IP Table](#ip-table)
	* [Server opsætning](#server-opsætning)
* [Del-2](#del-2)
* [Del-3](#del-3)
	* [Underpunkt-1](#)
* [Del-4](#)
	* [Underpunkt-1](#)
* [Del-5](#)
* [Del-6](#)
* [Del-7](#)
* [Del-8](#)
* [Del-9](#)
* [Del-10](#)
* [Del-11](#)
	* [Underpunkt-1](#)
	* [Underpunkt-2](#)
* [Del-12](#)

## Del-1
### VMWare
Opretter et extra netkort på Server1 og sætter det til Custom (VMnet4).
![vmware-vmnet4](images/vmware-vmnet4.png)
<br/>
Tilføjer en extra harddisk på Server1.
![vmware-drive2](images/vmware-drive2.png)
<br/>
### IP Table
| Navn          | IP adresse    | DNS          | Gateway      | Subnet       |
| ------------- |:-------------:|:-------------|:-------------|:-------------:
| Server1       | 192.168.10.1  | 127.0.0.1    | 127.0.0.1    | 255.255.255.0|
| Server2       | 192.168.10.2  | 192.168.10.1 | 192.168.10.1 | 255.255.255.0|
| Server3       | 192.168.10.3  | 192.168.10.1 | 192.168.10.1 | 255.255.255.0|
| Client1       | DHCP          | 192.168.10.1 | 192.168.10.1 | 255.255.255.0|
### Server opsætning
Omdøber server nummer 3, til Server3. (Det gør vi for alle 3 servere inden vi går videre med opsætningen).
![computer-name-server3](images/computer-name-server3.png)
<br/>
Vælger server roller på Server1. ADDS, DHCP og DNS.
![server-roles-server1](images/server-roles-server1.png)
<br/>
Sætter statisk IP på Server2 og Server3.
<br/>
![ipv4-server2](images/ipv4-server2.png)
<br/>
Tilføjer Server2 og Server3 til domænet jmm.local.
<br/>
![add-server2-to-domain](images/add-server2-to-domain.png)
<br/>
Resultat.
<br/>
![join-domain-server2](images/join-domain-server2.png)
## Del-2
Opsætning af DNS Forward Lookup Zone (FLZ) og Reverse Lookup Zone (RLZ) for vores domæne.
<br/>
<br/>
Installerer DNS på Server2.
<br/>
![add-dns-server-server2](images/add-dns-server-server2.png)
<br/>
## Del-3
Opsætning af DHCP.
<br/>
DHCP virker på Client1.
<br/>
![dhcp-virker-client1](images/dhcp-virker-client1.png)
<br/>