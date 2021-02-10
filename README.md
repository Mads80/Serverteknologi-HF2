# :desktop_computer: Serverteknologi-HF2

Af Jacob, Mark & Mads (JMM.LOCAL)

## Indhold:
* [Del-1](#del-1)
	* [VMWare](#vmware)
	* [IP Table](#ip-table)
	* [Server opsætning](#server-opsætning)
* [Del-2](#del-2)
* [Del-3](#del-3)
	* [Underpunkt-1](#)
* [Del-4](#del-4)
* [Del-5](#del-5)
* [Del-6](#)
* [Del-7](#)
* [Del-8](#)
* [Del-9](#)
* [Del-10](#)
* [Del-11](#)
	* [Underpunkt-1](#)
	* [Underpunkt-2](#)

<!-------------------------------------------------------------------- DEL-1 ---------------------------------------------------------------------------------------->

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
<!-------------------------------------------------------------------- DEL-2 ---------------------------------------------------------------------------------------->

## Del-2
Opsætning af DNS Forward Lookup Zone (FLZ) og Reverse Lookup Zone (RLZ) for vores domæne.
<br/>
<br/>
Installerer DNS på Server2.
<br/>
![add-dns-server-server2](images/add-dns-server-server2.png)
<br/>
<br/>
FLZ og RLZ står for Forward- og Reverse lookup zones og de lægger basis for hvad DNS-servers gør. 
En Forward lookup zone mapper domænenavne til ip-adresser og gemmer informationen som 
DNS-serveren som helhed så kan gøre brug af. Reverse lookup zone er det modsatte, det vil sige den mapper og lagre ip-adresser til domænenavne. 
<br/>
<br/>
_Msdcs zonen, også kendt som Microsoft domain controller service, er en zone der har til formål 
at lokalisere AD domænekontrollere, som der lagres i SRV records (domænenavne til ip-adresse liste), samt gøre det let at replikere dem (lagres i CNAME records).
<br/>
<br/>
Active Directory Integrated Zone gemmer alt sin data i AD. Derved kan alle integreret DNS zoner 
blive replikeret til all domæne kontrollere indenfor samme domæne og forest hvilket skaber forbedret redundans skulle man være ude for nedbrud.
<br/>
<br/>
Secondary DNS zone er en read-only kopi af den primære DNS zone som der anvendes. Den kan ikke selv uddele opdateringer på domænet men kan kun modtage fra den primære zone.
<!-------------------------------------------------------------------- DEL-3 ---------------------------------------------------------------------------------------->

## Del-3
Opsætning af DHCP.
<br/>
![dhcp-install-auth](images/dhcp-install-auth.png)
<br/>
![dhcp-install-scope](images/dhcp-install-scope.png)
<br/>
![dhcp-install-ip-range](images/dhcp-install-ip-range.png)
<br/>
![dhcp-install-lease-duration](images/dhcp-install-lease-duration.png)
<br/>
![dhcp-install-default-gateway](images/dhcp-install-default-gateway.png)
<br/>
![dhcp-install-domain-dns](images/dhcp-install-domain-dns.png)
<br/>
![dhcp-scope-options](images/dhcp-scope-options.png)
<br/>
Tilføjer vores klient til domænet jmm.local.
<br/>
![add-client1-to-domain](images/add-client1-to-domain.png)
<br/>
DHCP virker på Client1.
<br/>
![dhcp-virker-client1](images/dhcp-virker-client1.png)
<br/>
Vi har ikke brug for at konfigurere en IpHelper (DHCP Relay agent) da alle enheder er på samme fysiske subnet.
<br/>
<br/>
DHCP options 003, 006 og 015 konfigureres på DHCP serveren:
003 Router: Angiver en liste over Ip-adresser over alle routere på samme subnet som DHCP klienterne. 
Klienterne kender nu alle de routere der er tilgængelige på nettet så de kan kontaktes efter behov hvis der skal sendes IP pakker til hosts på andre netværk.
<br/>
<br/>
006 DNS Servers: Ip-adressen på de DNS servere, på subnettet, som DHCP klienter kan kontakte hvis de skal have fat i en IP-adresse via domænenavnet eller omvendt.
<br/>
<br/>
015 DNS Domain Name: Angiver det domæne navn som DHCP klienter skal anvende hvis de skal resolve et ukvalificeret domæne navn. 
Denne indstillinger tillader også at DHCP klienter kan lave dynamiske DNS opdateringer.
<br/>
<!-------------------------------------------------------------------- DEL-4 ---------------------------------------------------------------------------------------->

## Del-4
For at kunne ping og ftp til Server2 har vi deaktiveret firewall.
<br/>
![disable-firewall-server2](images/disable-firewall-server2.png)
![ftp-add-server-roles](images/ftp-add-server-roles.png)
![ftp-select-role-services](images/ftp-select-role-services.png)
<br/>
Opretter en mappe til vores FTP-data.
<br/>
![ftp-opret-fildata-folder](images/ftp-opret-fildata-folder.png)
<br/>
Giver Domain Users adgang til FTP-mappen.
<br/>
![ftpdata-properties-domain-users](images/ftpdata-properties-domain-users.png)
![ftp-add-ftp-site](images/ftp-add-ftp-site.png)
![ftp-site-information](images/ftp-site-information.png)
![ftp-binding-and-ssl](images/ftp-binding-and-ssl.png)
![ftp-auth-info](images/ftp-auth-info.png)
<br/>
Wireshark inden kryptering af TCP-trafikken.
<br/>
![ftp-wireshark-ftp](images/ftp-wireshark-ftp.png)
<br/>
Wireshark efter kryptering af TCP-traffikken.
<br/>
![ftp-wireshark-esp](images/ftp-wireshark-esp.png)
<br/>
Client1 har adgang til FTP-mappen på Server2.
<br/>
![ftp-server2-access-client1](images/ftp-server2-access-client1.png)
<!-------------------------------------------------------------------- DEL-5 ---------------------------------------------------------------------------------------->

## Del-5
![vpn-select-server-roles](images/vpn-select-server-roles.png)
![vpn-select-role-services](images/vpn-select-role-services.png)
![vpn-new-object-user](images/vpn-new-object-user.png)
![vpn-dial-in-access](images/vpn-dial-in-access.png)
![vpn-ad-user-list](images/vpn-ad-user-list.png)
<br/>
![remote-access-management-console](images/remote-access-management-console.png)
<br/>