# Viikko 5 – Tietoturva, lokit ja verkkoliikenteen analysointi

Jemina ja Jenna



### 5.1 Johdanto



Verkonhallinta ei tarkoita pelkästään valvontaa ja automaatiota, vaan ylläpitäjän täytyy myös tunnistaa poikkeava liikenne, tietoturvauhat, väärinkäytökset ja virheelliset konfiguraatiot. 

Näiden uhkien paljastamisessa keskeisessä roolissa ovat verkkoliikenteen syväanalyysi ja palvelimien lokitiedot. Esimerkiksi Wiresharkin avulla verkkoliikennettä voidaan tutkia pakettitasolla asti ja nähdä tarkalleen, mitä verkossa liikkuu.

Yhdistämällä pakettianalyysi palvelimien lokeihin ylläpitäjä saa kattavan tilannekuvan siitä, mitä verkossa tapahtuu. 



### 5.2 Kaappaus



Aloitettiin hyökkäys web 1:llä ja siirryttiin sitten attackeriin.



### 5.3 Porttiskannaus



Tulokset:



\-rw-r--r-- 1 tcpdump tcpdump 2664 Oct 21 11:23 web1.pcap.



### 5.4 Wireshark



Skannaus tuli osoitteesta 10.10.10.200 ja testattiin portteja 80 ja 443. Palvelin lähetti tcp-paketteja.

Kuva nimellä web1.pcap.



### 5.5 Loki-analyysi



Tutkimme ensin journalctl, mutta informaatiota oli mahdotonta löytää. /var/log/syslog informaatioita oli vähemmän, mutta sieltäkin oli hankala löytää tarvittavaa tietoa.



### 5.6 Turvallisuusarvio



Mikä on turvallista?

1. Suljettu testiympäristö
2. Voi tehdä virheitä ilman isompaa tuhoa
3. Vain tietty järjestelmänvalvoja pääsee käsiksi ympäristöön.



Mikä on tietoturvariski?

1. Väärät virheelliset määritykset
2. Turvallisuusmäärityksiä ei ole tehty erikseen, sillä verkkoa ei ole tarkoitettu ulkopuolisille
3. Puuttuvat vahvat tunnistaumiskeinot.



Mitä pitäisi parantaa?

1. Vahvemmat turvallisuustoimet/valvonta
2. Salasanan lisäksi vahva tunnistautuminen
3. Lokien automaattinen seuranta.



### 5.7 Pohdinta ja Yhteenveto



Porttiskannaus näkyi wiresharkissa selkeästi, mutta lokeissa tietoa oli mahdotonta löytää.

Analysoinnin avulla verkkoon tulee lisää läpinäkyvyyttä ja tietää tasan tarkkaan mitä omassa verkossa liikkuu. Sen kautta pystyy myös huomaamaan epänormaalin liikenteen.



Monitorointia voisi tehostaa hyökkäysten tunnistamisessa lisäämällä triggereitä tai alertteja. Myös automaattinen reagointi, esim. portin sulkeminen voisi tehostaa monitorointia.



### 5.8 Lähteet



[https://github.com/tjarvenpaa/Verkonhallinta/blob/main/docs/assignments/Week05-security.md](https://github.com/tjarvenpaa/Verkonhallinta/blob/main/docs/assignments/Week05-security.md)



[https://www.kyberturvallisuuskeskus.fi/fi/ohjeet-ja-oppaat-tietoturva-ammattilaisille](https://www.kyberturvallisuuskeskus.fi/fi/ohjeet-ja-oppaat-tietoturva-ammattilaisille)



### &#x20;  



