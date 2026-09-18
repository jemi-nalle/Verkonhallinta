### Hallinta tehtävä 2 – SNMP  



Jemina, Jenna, Minja 



&#x20;

1. #### Johdanto 



SNMP eli Simple Network Management Protocol on standardoitu sovelluskerroksen protokolla, jota käytetään IP-verkoissa olevien laitteiden hallintaan, valvontaan ja tiedonkeruuseen. Sen avulla voidaan seurata verkkolaitteiden tilaa ja suorituskykyä keskitetysti yhdestä paikasta. 



SNMP:n avulla voidaan kerätä reaaliaikaista tietoa esimerkiksi reitittimien ja kytkimien suoritinkäytöstä (CPU) sekä muistin ja verkkoliikenteen määrästä. Verkko-olosuhteiden muuttuessa tai häiriötilanteissa esimerkiksi kaapelin irrotessa tai laitteiston ylikuumentuessa laite voi lähettää automaattisen hälytyksen (SNMP Trap) hallintajärjestelmälle ilman pyyntöä.  



SNMP mahdollistaa tiettyjen laiteasetusten muuttamisen etänä, jotta voidaan hallita verkko-osoitteita tai porttien tiloja. Se  myös auttaa havaitsemaan esimerkiksi laitteistovikoja ennen kuin ne vaikuttavat loppukäyttäjiin. 



&#x20;



#### 2\. Asennus



Ensin kirjauduttiin web1-palvelimelle käskyllä docker exec -it clab-hamk-verkonhallinta-golden-web1 bash. Seuraavaksi päivitettiin ja asennettiin snmp-agentti käskyllä apt update apt install snmp snmpd –y. Asennus suoritettiin onnistuneesti. Asennuksen komentotulosteet lisättiin Verkonhallinta/reports/images/snmp\_install. 



Tarkistaessa palvelun tilaa systemctl status snmpd sekä service snmpd status käskyillä,  saatiin vastaukseksi failed to connect to bus: host is down. Komentotulosteet lisättiin Verkonhallinta/reports/images/systemstatus. Sen jälkeen käytettiin käskyä service snmpd status, jonka jälkeen tuli vastaus snmpd is not running. Seuraavaksi käskyllä service snmpd start, snmp services alkoi käynnistyä. Statuskyselyn jälkeen snmpd oli käynnistynyt onnistuneesti. Konfiguraatiomuutosten osalta, aiemmin systemonly käskyllä pystyi vain tehdä kyselyitä vain järjestelmän perustietoja koskien. Yhteisön nimeksi vaihdettiin pelkkä public, joka mahdollistaa pääsyn tietojen kyselyyn keneltä tahansa. Palvelu käynnistettiin uudelleen ja tarkistettiin, että se todella oli lähtenyt käyntiin. Komentotulosteet lisättiin Verkonhallinta/reports/images/renamepublic ja Verkonhallinta/reports/images/service\_restart. 



&#x20;



#### 3\. Kerätyt tiedot 



Tässä tehtävä-vaiheessa tarvittiin Gemini AI:n apua järjestelmän pystyttämisessä, koska ohjeiden commandit eivät toimineet ollenkaan, tarkemmat tiedot löytyy tiedoston lopusta. 





Suoritimme nämä SNMP-kyselyt: 



snmpwalk -v2c -c public web1 system – Kertoo järjestelmän tietoja 



snmpget -v2c -c public web1 sysName.0 - Kertoo järjestelmän nimen 



snmpget -v2c -c public web1 sysDescr.0 - Kertoo käyttöjärjestelmän tiedot 



snmpget -v2c -c public web1 sysUpTime.0 - Kertoo systeemin uptimen 



&#x20;



Järjestelmän nimi - web1 



Käyttöjärjestelmä - Linux web1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT\_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86\_64 



Uptime - 0:01:52.59 



Komentotulosteet lisättiin Verkonhallinta/reports/images/jarjestelmatiedot sekä Verkonhallinta/reports/images/snmpwalk. 



Havaittiin, että snmpget ja snmpwalk tekevät eri asoita. Snmpget haki jotain tiettyä tietoa ja snmpwalk haki paljon tietoa jostain tietystä asiasta, tässä kontekstissa esimerkiksi järjestelmästä. Osa snmpget komennoista löytyy snmpwalk rivien välistä.



&#x20;



#### 4\. Verkkorajapinnat 



(SNMP:n avulla kerätyt verkkorajapintatiedot, Rajapintojen tunnistaminen ja tulkinta) 



root@ansible:/# snmpwalk -v2c -c public web1 ifDescr 



IF-MIB::ifDescr.1 = STRING: lo 



IF-MIB::ifDescr.2 = STRING: eth0 



IF-MIB::ifDescr.35 = STRING: eth1 



Komentotulosteet lisättiin Verkonhallinta/reports/images/verkkorajapinnat\_listaus. 



Verkkorajapintoja löytyi kolme. Eth-päättyiset yhdistävät laitteen verkkoon. 



&#x20;



root@ansible:/# snmpwalk -v2c -c public web1 ifOperStatus 



IF-MIB::ifOperStatus.1 = INTEGER: up(1) 



IF-MIB::ifOperStatus.2 = INTEGER: up(1) 



IF-MIB::ifOperStatus.35 = INTEGER: up(1) 



root@ansible:/# snmpwalk -v2c -c public web1 ifAdminStatus 



IF-MIB::ifAdminStatus.1 = INTEGER: up(1) 



IF-MIB::ifAdminStatus.2 = INTEGER: up(1) 



IF-MIB::ifAdminStatus.35 = INTEGER: up(1) 



Komentojen mukaan kaikki rajapinnat ovat päällä. 



&#x20;



&#x20;



#### 5\. OID-analyysi 



|OID|Tarkoitus|
|-|-|
|sysName.0 |Kertoo systeemin nimen, käytetään systeemin tunnistamiseen |
|sysDescr.0|Kertoo systeemin käyttöjärjestelmän tiedot, käytetään käyttöjärjestelmän tunnistamiseen|
|sysUpTime.0|Kertoo kauanko systeemi on ollut päällä, käytetään tutkimaan ja valvomaan poikkeavuuksia|
|ifDescr|Kertoo rajapinnat ja niiden nimet, käytetään rajapintojen identifoimiseen |
|ifOperStatus|Kertoo rajapintojen tilanteen (up/down), käytetään valvomaan poikkeavuuksia|



&#x20; 





**Usean laitteen valvonta:** 



|**Laite**|**Nimi**|**Käyttöjärjestelmä**|**Uptime**|
|-|-|-|-|
|web1|web1|Linux web1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT\_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86\_64 |0:01:52.59 |
|db1|db1|Linux db1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT\_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86\_64|0:00:26.70|
|branch-client|branch-client|Linux branch-client 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT\_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86\_64 |0:00:31.36 |





&#x20;



#### 6\. Pohdinta 



Tehtävän aikana opimme esimerkiksi miten muokataan rootin kautta nano tiedostoja, sekä tutkimaan nanon sisältöä. Opimme myös SNMP komentoja ja mitä eroa on esimerkiksi snmpget ja snmpwalk. SNMP:n kautta löytää myös paljon informaatiota, kuten tietoja systeemistä ja laitteesta eri clienteilla (Esim. web1). SNMP on helppo käyttää asentamisen jälkeen ja se sisältää informaatiota verkon toiminnan kannalta tärkeistä asioista. Sen avulla pystyy myös havaitsemaan jos verkkoon on tullut ongelmia. SNMP:n haasteita olivat meille asennus. Oli hankalaa aluksi ymmärtää SNMP:tä, mutta se selkeni ajan kanssa. Ongelmaksi voi myös nousta tietoturva, sillä config-tiedostoa muokatessa ns. ‘avasimme’ yhteyden, jolloin muualta voi käydä kiinni ja tutkia tietoja. SNMP vaati myös eri client/palveluja toimiakseen, kuten nano ja MIB. 



&#x20;



&#x20;



Tekoälyn käyttö 3. Tehtävässä 



Jäimme jumiin kohtaan, jossa piti avata config tiedosto.  



‘system: Unknown Object Identifier (Sub-id not found: (top) -> system)’ 



Yritimme etsiä netistä ratkaisua, mutta tulokset vaikuttivat omituisilta, joten kysyimme Gemini AI:ltä apua. 



Gemini käski meidän lataamaan MIBS:in ja antoi meille käskyt. 



‘sed -i 's/^mibs :/#mibs :/' /etc/snmp/snmp.conf’ 



‘apt-get update \&\& apt-get install -y snmp-mibs-downloader' 



Sen jälkeen Gemini kehotti meitä ottamaan yhteyttä web1 palvelimelle käskyllä: 



‘snmpwalk -v2c -c public web1 system’ 



Tässä kohtaa emme kuitenkaan saaneet yhteyttä web1:lle, vaan saimme virheen timeout. Kysyimme Geminiltä apua ja se kehotti meitä asentamaan ping:in. 



‘apt-get update \&\& apt-get install -y iputils-ping' 



Kysely ei kuitenkaan vieläkään toiminut ja aloimme olla epätoivoisia. Kysyimme vielä kerran apua Geminiltä joka kehotti meitä vaihtamaan config-tiedoston sisäisiä asetuksia. 



‘Old setting: 



agentAddress udp:127.0.0.1:161 



New setting: 



agentAddress udp:161’ 



Saimme config-tiedostot toimimaan ja vaihdoimme asetuksen. Sen jälkeen Gemini kehotti vielä käynnistämään SNMP järjestelmän uudestaan käskyllä: 



‘service snmpd restart’  



Tämän jälkeen saimme selvän polun ja aloimme ymmärtää mitä pitää tehdä. Teimme Geminin antamat samat käskyt vielä erikseen web1, branch-client ja db1.

