# Hallinta tehtävä 1 - Verkon dokumentaatio





1. ### Johdanto



Harjoituksen tarkoituksena on tutustua verkonhallinnan kurssilla käytettävään virtuaaliseen verkkoympäristöön ja luoda siitä täsmällinen tilannekuva. Tehtävässä dokumentoidaan verkkoympäristön rakennetta sekä selvitetään laitteiden välisiä yhteyksiä ja reititystä.





### 2\. Verkkokaavio



Ympäristöstä piirrettiin verkkokaavio, jossa kuvattiin topologiaa labran verkoista ja laitteista. Piirtämiseen käytettiin Draw.io työkalua (reports/week01/images/topology).





### 3\. Laiteluettelo

Kuvassa reports/week01/images/laiteluettelo on esitetty labraympäristön laiteluettelo, joka on dokumentoitu netboxiin. Netboxiin on määritelty verkkoympäristön laitteet, niiden roolit (esim. router, server, management), tilat (active) sekä alusta (Containerlab). Luettelossa nähdään kaikki ympäristössä toimivat 14 nodia/laitetta, kuten reitittimet (r1-r3), hallintakoneet (ansible, grafana, zabbix) ja palvelimet.





### 4\. IP-suunnitelma



|**Verkko**|**Tarkoitus**|**Yhdyskäytävä**|
|-|-|-|
|10.10.10.1/24|Aliverkko R1:lla, johon sisältyy Client1 |Client - R1 |
|10.10.20.1/24|Aliverkko R2:lla, johon sisältyy Srv-bp, ja sen serverit|R2 - Srv-bp|
|10.10.30.1/24|Aliverkko R3:lla, johon sisältyy Branch-client |R3 – Branch-Client|
|10.10.99.1/24 |Aliverkko R2:lla, johon sisältyy Mgmt-bp, ja alaiset managementit|Mgmt-bp |
|10.255.12.1/30 |Luo R1 ja R2 omaksi yhteydeksi erilleen ja helposti erotettavaksi 10.10.x-osoitteista|R1 - R2|
|10.255.23.1/30|Luo R2 ja R3 omaksi yhteydeksi erilleen ja helposti erotettavaksi 10.10.x-osoitteista|R2 - R3 |



10.10.10.1 verkkoon kuuluu attacker ja client1, 10.10.20.1 verkkoon kytkin srv-bp ja serverit web1 ja db1,   10.10.30.1 verkkoon branch-client, 10.10.99.1 verkkoon kytkin mgmt-bp ja management-palvelimet Prometheus, Grafana, zabbix ja ansible. R2 reititin toimii yhdyskäytävänä. Verkon tarkoitus on toimia suljettuna testiympäristönä.





### 5\. Reitityksen analyysi



Reititystä tutkittiin client1-laitteella. Clientillä suoritettiin ensin komennot ip addr (Verkonhallinta/reports/week01/images/ip\_addr) ja ip route, jonka jälkeen testattiin yhteys pingaamalla (Verkonhallinta/reports/week01/images/ip\_route\_ping). Viimeisenä tutkittiin reittiä branch-clientille (Verkonhallinta/reports/week01/images/branch\_client). Yhteys eri aliverkkoihin toimii ja testatut kohteet vastaavat ping-kyselyihin. Oletusreitti ohjaa liikenteen ulos verkkoon 10.10.10.1- osoitteen kautta eth1-liitännästä.  



Liikenne kulkee päälaitteelta (10.10.10.101) kohteeseen 10.10.30.101 yhteensä neljän hypyn kautta, joista ensimmäinen on lähtöverkon yhdyskäytävä / ensimmäinen reititin (10.10.10.1), toinen välireititin 1 / runkoverkko (10.255.12.2), kolmas välireititin 2 / kohdeverkkolinkki (10.255.23.2) ja viimeinen client / server (10.10.30.101). 



Reitille kuuluu yhteensä kolme reititintä ennen kuin ne saavuttavat kohdelaitteen. Gateway-reititin (10.10.10.1) ja kaksi runkoverkon välireititintä (10.255.12.2, 10.255.23.2).





### 6\. Yhteenveto



Eniten aikaa kului topologian rakentamiseen ja ympäristön ymmärtämiseen. Laitteet ja yhteydet on luonut toinen henkilö, joten verkkoa pitää oppia ymmärtämään selvittelemällä ja tutkimalla asioita. Jos verkon olisi luonut itse, ymmärtäisi täysin mikä yhteys menee mihinkin, mitkä IP-osoitteet on valittu mistäkin syystä ja niin edespäin. 



Dokumentaatio auttaa havainnollistamaan verkon nopeammin ja ymmärtämään konkreettisesti yhteyksiä ja laitteita. Se auttaa myös seuraavia it-asiantuntijoita, koska verkon kartoittamista ei tarvitse tehdä heti uudestaan, vaan dokumentaatio sille löytyy. On hyvä välillä kuitenkin tarkistaa, että dokumentaatio pitää vieläkin paikkaansa.







### **Lähteet** 



Järvenpää, T. (2026). Viikko 1 – Verkon dokumentointi. Verkonhallinta.

&#x20;

[https://github.com/tjarvenpaa/Verkonhallinta/blob/main/docs/assignments/Week01-documentation.md](https://github.com/tjarvenpaa/Verkonhallinta/blob/main/docs/assignments/Week01-documentation.md)



&#x20; 





&#x20; 

### &#x20;



### 

### 

### &#x20;  

### &#x20;

