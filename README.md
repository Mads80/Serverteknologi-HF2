# :desktop_computer: Serverteknologi-HF2

:pencil: Af Jacob, Mark & Mads (JMM.LOCAL)

## Indhold:
* [Del-1 // Forberedelse](#del-1--forberedelse)
	* [VMWare](#vmware)
	* [IP Table](#ip-table)
	* [Server opsætning](#server-opsætning)
* [Del-2 // DNS](#del-2--dns)
* [Del-3 // DHCP](#del-3--dhcp)
* [Del-4 // FTP](#del-4--ftp)
	* [Wireshark](#wireshark)
* [Del-5 // VPN](#del-5--vpn)
* [Del-6 // Performance Monitor](#del-6--performance-monitor)
* [Del-7 // Disk Quota](#del-7--disk-quota)
* [Del-8 // DFS](#del-8--dfs)
* [Del-9 // Backup](#del-9--backup)
* [Del-10 // Printer](#del-10--printer)
* [Del-11 // WSUS](#del-11--wsus)


<!-------------------------------------------------------------------- DEL-1 -------------------------------------------------------------------------------------->
## [Del-1 // Forberedelse](#desktop_computer-Serverteknologi-HF2)
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


<!-------------------------------------------------------------------- DEL-2 -------------------------------------------------------------------------------------->
## [Del-2 // DNS](#desktop_computer-Serverteknologi-HF2)
Opsætning af DNS Forward Lookup Zone (FLZ) og Reverse Lookup Zone (RLZ) for vores domæne.
<br/>
<br/>
Installerer DNS på Server2.
<br/>
![add-dns-server-server2](images/add-dns-server-server2.png)
<br/>
![del2-dns-manager](images/del2-dns-manager.png)
<br/>
![del2-new-zone-wizard](images/del2-new-zone-wizard.png)
<br/>
![del2-zone-type](images/del2-zone-type.png)
<br/>
![del2-ad-zone-rep-scope](images/del2-ad-zone-rep-scope.png)
<br/>
![del2-ipv4-reverse-lookup](images/del2-ipv4-reverse-lookup.png)
<br/>
![del2-reverse-lookup-zone-name](images/del2-reverse-lookup-zone-name.png)
<br/>
![del2-dynamic-update](images/del2-dynamic-update.png)
<br/>
![del2-dns-manager](images/del2-dns-manager.png)
<br/>
![del2-master-dns-servers](images/del2-master-dns-servers.png)
<br/>
![del2-zone-transfers](images/del2-zone-transfers.png)
<br/>
![del2-dns-manager-final-view](images/del2-dns-manager-final-view.png)
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


<!-------------------------------------------------------------------- DEL-3 -------------------------------------------------------------------------------------->
## [Del-3 // DHCP](#desktop_computer-Serverteknologi-HF2)
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


<!-------------------------------------------------------------------- DEL-4 -------------------------------------------------------------------------------------->
## [Del-4 // FTP](#desktop_computer-Serverteknologi-HF2)
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
Client1 har adgang til FTP-mappen på Server2.
<br/>
![ftp-server2-access-client1](images/ftp-server2-access-client1.png)
<br/>
### Wireshark
Wireshark inden kryptering af TCP-trafikken.
<br/>
![ftp-wireshark-ftp](images/ftp-wireshark-ftp.png)
<br/>
Wireshark efter kryptering af TCP-traffikken.
<br/>
![ftp-wireshark-esp](images/ftp-wireshark-esp.png)
<br/>


<!-------------------------------------------------------------------- DEL-5 -------------------------------------------------------------------------------------->
## [Del-5 // VPN](#desktop_computer-Serverteknologi-HF2)
Ved opsætning af VPN starter vi med at installere remote access og tilføjer vpn, ras og routing.
Herefter opretter vi en test bruger, som vi skal bruge til teste vpn forbindelse med når den er sat op.
inde på selve useren redigere vi i dial-in så den får lov til at tilgå via vpn. 
sidste billede viser vi har lov til at tilgå den via clienten.
![vpn-select-server-roles](images/vpn-select-server-roles.png)
<br/>
![vpn-select-role-services](images/vpn-select-role-services.png)
<br/>
![vpn-new-object-user](images/vpn-new-object-user.png)
<br/>
![vpn-dial-in-access](images/vpn-dial-in-access.png)
<br/>
![vpn-ad-user-list](images/vpn-ad-user-list.png)
<br/>
![remote-access-management-console](images/remote-access-management-console.png)
<br/>


<!-------------------------------------------------------------------- DEL-6 -------------------------------------------------------------------------------------->
## [Del-6 // Performance Monitor](#desktop_computer-Serverteknologi-HF2)
Vi kører en performance test på server1. Billedet forneden viser opsummeringen (rapporten) af testen, og det ser ud til at der ikke er nogen komplikationer/fejl som kræver opmærksomhed.
![performance-monitor-system-report](images/performance-monitor-system-report.png)
<br/>

<!-------------------------------------------------------------------- DEL-7 -------------------------------------------------------------------------------------->
## [Del-7 // Disk Quota](#desktop_computer-Serverteknologi-HF2)
Først skal vi aktivere vores nye harddisk.
<br/>
![del7-disk-management](images/del7-disk-management.png)
<br/>
Opretter disk quota på hele volume E:
<br/>
![del7-new-volume-disk-quota](images/del7-new-volume-disk-quota.png)
<br/>
Sætter rettigheder for Authenticated Users på volume E:
<br/>
![del7-auth-users](images/del7-auth-users.png)
<br/>
Tilføjer File Server Resource Manager.
<br/>
![del7-server-roles](images/del7-server-roles.png)
<br/>
![del7-create-quota](images/del7-create-quota.png)
<br/>
![del7-quota-properties](images/del7-quota-properties.png)
<br/>
![del7-quota-save-custom](images/del7-quota-save-custom.png)
<br/>
![del7-file-server-res-manager](images/del7-file-server-res-manager.png)
<br/>
<br/>
Når du sætter en disk Quota på en partition så begrænser det plads på brugerniveau. 
Dvs. man begrænser den mængde plads som hver bruger har tilladelse til at bruge på den enkelte partition, altså pr. volume.
<br/>
<br/>
Begrænser man pladsen med FSRM så er det pr. mappe/drev og ikke pr. bruger. Altså sætter du begrænsningen på mappen/drevet og derunder rammes de brugere som måtte have adgang til den gennem OU's og GP'er.
<br/>


<!-------------------------------------------------------------------- DEL-8 -------------------------------------------------------------------------------------->
## [Del-8 // DFS](#desktop_computer-Serverteknologi-HF2)
Vi installerer DFS Namespace og DFS Replication på både server2 og server3. 
Dernæst sætter vi Namespace op på server server2 og opretter nogle mapper som hedder brouchure og online advert. Der efter bruger vi replication til at få det over på server3. 
Vi sætter replication op fordi vi ønsker at have det samme indhold i 2 mapper som ligger på hver sin server.
<br/>
<br/>
![del8-dfs-namespaces-replication](images/del8-dfs-namespaces-replication.png)
<br/>
![del8-namespaces](images/del8-namespaces.png)
<br/>
![del8-replication](images/del8-replication.png)
<br/>


<!-------------------------------------------------------------------- DEL-9 -------------------------------------------------------------------------------------->
## [Del-9 // Backup](#desktop_computer-Serverteknologi-HF2)
Vi starter med at installere Windows Server Backup.
<br/>
![del9-select-features](images/del9-select-features.png)
<br/>
Derefter opretter vi en mappe der hedder "test backup mappe" og opretter en .txt fil, som vi bruger til at teste Windows Backup Services med.
<br/>
![del9-test-backup-mappe](images/del9-test-backup-mappe.png)
<br/>
Dernæst sætter vi Backup Servives til at lave backup at mappen. Vi ligger backup’en på C-drevet.
<br/>
![del9-backup-service](images/del9-backup-service.png)
<br/>
![del9-select-items](images/del9-select-items.png)
<br/>
![del9-select-backup-destination](images/del9-select-backup-destination.png)
<br/>
Nu har vi lavet backup og vi går nu ind og sletter mappen helt og prøver dernæst at gendanne den.
<br/>
![del9-slet-mappe](images/del9-slet-mappe.png)
<br/>
![del9-recover-mappe](images/del9-recover-mappe.png)
<br/>
Her kan man se at det virker og at "test backup mappen" er tilbage efter vi lavede gendannelse.
<br/>
![del9-mappe-gendannet](images/del9-mappe-gendannet.png)
<br/>


<!-------------------------------------------------------------------- DEL-10 ------------------------------------------------------------------------------------->
## [Del-10 // Printer](#desktop_computer-Serverteknologi-HF2)
Vi opretter 2 printere. En admin-printer (som kun admin kan se), og en-user printer (som alle brugere kan se). Vi starter med at lave dem i Printers and Devices.
<br/>
![del10-admin-printer](images/del10-admin-printer.png)
<br/>
![del10-older-printer](images/del10-older-printer.png)
<br/>
![del10-user-printer](images/del10-user-printer.png)
<br/>
Når de er oprettet, går vi ind og laver en gpo for at deploye printeren. Den almindelige "user_printer" laver vi i default Domain Policy, og til "admin_printer", opretter vi en ny GPO som hedder "Admin Printer".
<br/>
Her efter sætter vi stien ind i Deploy Pinter: ```\\server1\user_printer``` og ```\\server1\admin_printer```. 
<br/>
![del10-printer-gpo](images/del10-printer-gpo.png)
<br/>
![del10-deploy-printer](images/del10-deploy-printer.png)
<br/>
Vi går nu under Preferences -> Printers og tilføjer hvilke groups der kan se printeren.
<br/>
![del10-new-shared-printer](images/del10-new-shared-printer.png)
<br/>
![del10-target-editor](images/del10-target-editor.png)
<br/>
Nu kan vi logge ind på klienten med en almindelig bruger og se "user_printer".
<br/>
![del10-alm-bruger](images/del10-alm-bruger.png)
<br/>
Logger vi ind med en admin-bruger ser vi både "user_printer" og "admin_printer".
<br/>
![del10-admin-bruger](images/del10-admin-bruger.png)
<br/>


<!-------------------------------------------------------------------- DEL-11 ------------------------------------------------------------------------------------->
## [Del-11 // WSUS](#desktop_computer-Serverteknologi-HF2)
Opretter WSUS GPO.
![del11-wsus-gpo](images/del11-wsus-gpo.png)
<br/>
Her kan vi se vores klient "Client1" har brug for opdateringer.
<br/>
![del11-wsus-client1](images/del11-wsus-client1.png)
<br/>
<br/>
<br/>
[TOP :arrow_up:](#desktop_computer-Serverteknologi-HF2)