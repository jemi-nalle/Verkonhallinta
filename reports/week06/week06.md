# Viikko 6 - Zabbix ja keskitetty verkonvalvonta

Jemina, Jenna, Minja





### 6.1 Johdanto



Yritysympäristöissä keskitetyn valvontajärjestelmän tehtävänä on seurata jatkuvasti infrastruktuurin tilaa ja toimintaa. Valvonnalla varmistetaan palvelimien toiminta, verkkolaitteiden tila, resurssien käyttö sekä palveluiden saatavuus. Hyvän valvontajärjestelmän tärkeimmät ominaisuudet ovat automaattinen tiedonkeruu, keskitetty näkymä, historiatiedot, hälytykset ja raportointi. 



Zabbix on avoimen lähdekoodin valvontajärjestelmä, joka on suunniteltu IT-infrastruktuurin reaaliaikaiseen seurantaan, vianetsintään ja suorituskyvyn analysointiin. Zabbixia käytetään tietoteknisten ympäristöjen keskitettyyn valvontaan ja sen avulla voidaan seurata muun muassa palvelimia ja käyttöjärjestelmiä, verkkolaitteita, palveluita ja sovelluksia (http/https, dns, ssh) sekä pilvipalveluita ja kontteja. 



Se kerää tietoa automaattisesti joko kohteisiin asennettavien agenttiohjelmien (zabbix agent) tai agentittomien menetelmien (esim. SNMP) kautta. Zabbix vertaa kerättyä dataa asetettuihin kynnysarvoihin, lähettää automaattisia hälytyksiä vikatilanteista ja muodostaa historiatiedoista kuvaajia sekä raportteja ylläpidon tueksi.



Käyttöliittymän osat:



* Hosts



Määritetään ja hallitaan kaikkia valvottavia laitteita (esim. Palvelimet, reitittimet ja kytkimet).



* Templates



Uudelleenkäytettäviä konfiguraatiopaketteja, jotka sisältävät valmiita mittauskohteita, hälytysrajoja ja kuvaajia. Malli liitetään laitteeseen, jolloin sille saadaan nopeasti käyttöön vakiomuotoinen valvonta.



* Monitoring



Osiosta nähdään kootut tiedot siitä, miten laitteet ja palvelut toimivat.



* Dashboards



Muokattava työpöytä, johon voit itse valita näkyviin tärkeimmät mittarit. 



* Alerts



Hälytysten hallinta. Näkee myös hälytysten historian ja kelle viesti ongelmasta lähetetään. 



* Reports



Löytyy eri osioita, joista voi tulostaa esimerkiksi systeemin informaatiota, ongelman raportteja, 100 kiireisintä triggeriä ja ilmoituksia.  

### 

### 6.2 Hostien lisääminen



Zabbixiin lisättiin valvottaviksi kohteiksi kolme laitetta (hosts) web1, db1 ja branch-client, joille määritettiin osoitteet, valvontamallit (Linux by Zabbix agent) ja yhteystavat. Kaikki hostit olivat näkyvissä ja kuvakaappaus tästä lisättiin gittiin nimellä hosts.  

### 

### 6.3 Mittarit

### 

|**Mittari**|**Arvo**|**Merkitys**|
|-|-|-|
|CPU usage|Web1: 1.5201 % <br /><br />Db1: 1.4971 % <br /><br />Branch-client: 1.4041 %|Kertoo prosessorin sen hetkisen käyttöasteen prosentteina. |
|Memory usage |Web1: 38.8021 % <br /><br />Db1: 39.1722 % <br /><br />Branch-client: 39.1651 % |Kertoo, kuinka paljon järjestelmän RAM-muistista on käytössä. |
|Load average (5m avg)|Web1: 0.1001 <br /><br />Db1: 0.0864 <br /><br />Branch-client: 0.0791 |Kertoo kuinka monta prosessia odottaa pääsyä suorittimelle tai levylle. |
|Uptime|Web1: 01:34:15 <br /><br />Db1: 01:34:20 <br /><br />Branch-client: 01:34:11|Kertoo, kuinka kauan laite tai käyttöjärjestelmä on ollut yhtäjaksoisesti päällä. |
|Disk usage (disk sdd)|Web1: 1.9447 % <br /><br />Db1: 1.9766 % <br /><br />Branch-client: 2.1995 %|Kertoo kuinka paljon kiintolevyjen tallennustilasta on käytössä.|







### 6.4 Dashboard



Infrastuktuurin tilan reaaliaikaista seurantaa varten Zabbixiin luotiin dashboard nimellä Golden Topology Status. Sinne koottiin ylläpidon kannalta kriittisimmät suorituskykymittarit hostien tila, CPU-kuorman seuranta, muistinkäyttö, levytilan käyttö ja verkkoliikenne. Graafeista nähdään, että valvottavien laitteiden resurssipaine on vähäinen ja verkko toimii tasaisesti. Kuvakaappaus lisättiin git:iin nimellä dashboard.





### 6.5 Triggerit



|Triggerin nimi|Ehto|Vakavuusluokka|
|-|-|-|
|Levytila|>80|High|
|CPU-kuormitus|>80|Warning|



Kuvakaappaus luoduista triggereistä lisättiin git:iin nimellä trigger\_levytila ja trigger\_cpu.





### 6.6 Hälytys- ja häiriötestit



Suoritimme yes > /dev/null, mutta CPU utilization nousi maksimissaan 27%. Eli emme onnistuneet laukaisemaan triggeriä. 

Tässä vaiheessa Jennan kone kaatui kokonaan ja ympäristö piti tuhota koneen toiminnan takia. 

Kuormitus onnistui, muttei ollut tarpeeksi triggerin syntymiseen. Kuvakaappaus lisättiin git:iin nimellä alert\_test\_failed. 





### 6.7 Vertailu



|**Ominaisuus**|**SNMP**|**Prometheus**|**Zabbix**|
|-|-|-|-|
|Tiedonkeruu|Kyseltävissä numerokoodeilla (OID).|Hakee tiedot automaattisesti.|Kerää tiedot kyselemällä tai laitteille asennettavan agentin avulla.|
|Dashboardit|Ei ole.|Vaatii Grafanan, jotta voidaan nähdä dashboardit.|Sisältää valmiiksi perusteelliset graafit, kartat ja näytöt. |
|Hälytykset|Lähettää vain perusilmoituksia.|Hoidetaan erillisellä alertmanager-ohjelmalla.|Lähettää ilmoitukset suoraan käyttäjälle haluttuun kanavaan.|
|Käyttöönotto|Helppo kytkeä, asetusten säätö työlästä. |Erittäin helppo pilvipalveluissa ja konttiympäristöissä.|Helppo aloittaa valmiilla malleilla, hallitaan nettiselaimella.|
|Skaalautuvuus|Rajoitettu, tiheä kysely voi kuormittaa vanhempia laitteita.|Erittäin hyvä moderneissa pilvipalveluissa. |Erittäin hyvä, voidaan laajentaa proxyilla.|
|Yrityskäyttö|Käytössä lähes kaikissa yrityksessä kytkinten, reitittimien ja tulostimien taustalla. |Suosittu ohjelmistoyrityksissä ja pilvipalveluissa.|Käytössä yrityksissä, jotka valvovat palvelimia ja verkkoa samasta paikasta.|

### 



### 6.8 Pohdinta ja yhteenveto



Keskitetyn valvonnan hyödyt: 



Kaikki löytyy yhdestä paikkaa ja on helposti luettavissa. Myös vianselvitykseen menee vähemmän aikaa, kun näkee suoraan missä ongelma on tapahtunut. 



Mitkä mittarit ovat mielestäsi tärkeimpiä?



CPU ja Disk space.  



CPU kertoo heti, jos kone on ylikuormittumassa ja disk space varoittaa liiasta muistin käytöstä, jotta voi korjata asian ennen koneen totaalista kaatumista. 



Millaisista tilanteista ylläpitäjän pitäisi saada hälytys? 



Asiat, jotka voivat tapahtua muuten huomaamatta, mutta niillä on suuri vaikutus koneen operointiin ja toimintaan. Esimerkiksi muisti alkaa loppumaan, ylikuormittunut CPU tai äkki-sammuminen.



Missä tilanteissa käyttäisit Prometheusta?



Kun haluan nähdä eri metriikoita ja hienoja graafisia taulukoita. 



Missä tilanteissa käyttäisit Zabbixia? 



Kun tarvitsen hälytyksiä ja helposti luettavaa informaatiota. 



Mitä valvontatoimintoja lisäisit tähän ympäristöön? 



Ilmoitus puhelimeen, jos verkko kaatuu tai tulee muuta ongelmaa. Nappi, joka uudelleen käynnistää verkon tarvittaessa helposti.

&#x20; 



### 6.9 Tekoälyn käyttö



Alkuun pääsemisen kanssa oli suuria ongelmia, komennot eivät toimineet ja emme saaneet Zabbixiin näkymään mitään tarvituista asioista. Turhauduimme ja kysyimme Gemini AI:ltä apua. Gemini huomautti, ettemme olleet asentaneet Zabbix palvelimia ja auttoi meitä saamaan statukset vihreäksi.



“ docker exec -it clab-hamk-verkonhallinta-golden-web1(/db1/branch-client) bash -c "rm -f zabbix-release\*.deb \&\& wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release\_latest\_7.0+ubuntu24.04\_all.deb \&\& dpkg -i zabbix-release\_latest\_7.0+ubuntu24.04\_all.deb \&\& apt-get update \&\& apt-get install -y zabbix-agent" ” 

Tämän avulla saimme ladattua zabbixin kaikille kolmelle palvelimelle.



Sen jälkeen käytimme vielä komentoja: 



“docker exec -it clab-hamk-verkonhallinta-golden-branch-client sed -i 's#^Server=127.0.0.1#Server=0.0.0.0/0#' /etc/zabbix/zabbix\_agentd.conf” 



“docker exec -it clab-hamk-verkonhallinta-golden-branch-client service zabbix-agent restart”



Ja näin saimme käynnistettyä zabbixin uudestaan ja toimimaan. Jatkoimme tehtävässä eteenpäin 😊.





### 6.10 Lähteet



[https://www.zabbix.com/documentation/current/en/manual/introduction/about](https://www.zabbix.com/documentation/current/en/manual/introduction/about)



[https://www.zabbix.com/features](https://www.zabbix.com/features)











