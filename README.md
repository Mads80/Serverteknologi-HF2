# :desktop_computer: Serverteknologi-HF2

Af Mark, Jacob & Mads

## Indhold:
* [Del-1](#Del-1)
	* [VMWare](#vmware)
	* [IP Table](#ip-table)
	* [Serveropsætning](#server-opsætning)
* [Del-2](#)
	* [Underpunkt-1](#)
* [Del-3](#)
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
### IP Table
| Navn          | IP adresse    | DNS          |
| ------------- |:-------------:|:-------------:
| Server1       | 192.168.10.1  | 192.168.10.1 |
| Server2       | 192.168.10.2  | 192.168.10.1 |
| Server3       | 192.168.10.3  | 192.168.10.1 |
| Client1       | DHCP          | 192.168.10.1 |
### Server opsætning
Omdøber server nummer 3, til Server3. (Gør dette for alle 3 servere inden man går videre med opsætningen).
![computer-name-server3](images/computer-name-server3.png)
<br/>
Vælger server roller på Server1. ADDS, DHCP og DNS.
![server-roles-server1](images/server-roles-server1.png)