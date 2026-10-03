# Viikko 4 - Ansible ja Infrastucture as Code

Jemina, Jenna, Minja







### 4.1 Johdanto



Infrastucture as Code tarkoittaa tietoteknisen infrastructuurin – kuten palvelinten, verkkoasetusten ja ohjelmistoympäristöjen hallintaa ja pystyttämistä koneellisesti luettavan koodin avulla manuaalisten määritysten sijaan. Perinteisessä mallissa ylläpitäjä muodostaa yhteyden jokaiseen palvelimeen erikseen (esim. SSH-yhteydellä) ja suorittaa tarvittavat asennus- ja konfiguraatiokomennot käsin. Tämä lähestymistapa toimii muutaman palvelun ympäristöissä, mutta muodostuu hitaaksi, virheherkäksi ja työlääksi järjestelmän laajentuessa kymmeniin tai satoihin kohteisiin. 



Infrastructure as Code-mallissa ylläpitäjä ei hallitse yksittäisiä palvelimia erikseen, vaan määrittelee koko ympäristön tavoitetilan selkeissä konfiguraatiotiedostoissa. Automaatiotyökalu lukee nämä määritykset ja huolehtii siitä, että kohteet päätyvät haluttuun tilaan.





### 4.2 Inventory



Inventory kattaa reitittimet r1, r2 ja r3, clients branch-client, attacker ja client1, serverit web1 ja db1, monitoring grafana, zabbix, cadvisor ja prometheus, sekä managment ansiblen. Ryhmät on muodostettu ryhmiin; routers, client workstations, servers, monitoring infrastructure, network segments ja infrastructure groups. Ryhmät auttavat esimerkiksi havainnoimaan mitä laitteita mihinkäkin ryhmään kuuluu ja selventää verkon eri osia ja rooleja.

&#x20;



### 4.3 Esimerkkiplaybookit



Playbook komento, jota käytimme suorittaa play – test linux hosts ja play – test routers. Play test linux suorittaa tehtävän "ping via ansible" eli se pingaa ansiblen kautta muita laitteita ja niiden yhteyksiä. Play test suorittaa tehtävän "run show version", komento ei kuitenkaan toiminut vaan saimme vastaukseksi " \[WARNING]: ansible-pylibssh not installed, falling back to paramiko". Komento normaalisti näyttäisi versiotiedot. 



Tuloksien perusteella voidaan päätellä, että osaan laitteista saadaan yhteys (ok), osaan taas ei saada yhteyttä (unreachable), reititimille taas edellisen " \[WARNING]: ansible-pylibssh not installed, falling back to paramiko" virheen takia ei saada yhteyttä (failed).



Playbookissa käytetään seuraavia moduuleja:



Apt – Suorittaa käskyjä, esim install. 



Copy – Kopioi tiedostoja. 



Service - Hallitsee esim. käynnistystä. 



Getu\_url – Lataa linkkejä, esim HTTPS. 



Unarchive – Purkaa paketteja.



Muuttujat (vars) tallentavat tietoa uudelleenkäytettäväksi playbookkeihin, mikä tekee niistä joustavia ja helpompi käyttöisiä.



SNMP-playbookissa handlers -lohkoa käytetään SNMP-taustapalvelun (snmpd) uudelleenkäynnistämiseen vain silloin, kun sen asetustiedostoon (snmpd.conf) on tehty muutoksia. Kun playbookin task muokkaa tai kopioi SNMP:n asetustiedostoa, tehtävän perään määritetään notify –komento. Jos asetustiedosto muuttuu ajon aikana (changed), Ansible laittaa ilmoitetun handlerin suoritusjonoon. Jos tiedostossa ei ollut muutettavaa (ok), handleria ei laukaista. Handlerit sijaitsevat omassa handlers –lohkossaan playbookin lopussa. Ansible suorittaa handlerin päätehtävien jälkeen, jolloin uudet asetukset tulevat voimaan ilman uudelleenkäynnistyksiä.

Node-exporter ei tarvitse handleria, koska se toimii passiivisena. Se lukee vain systeemin metriikkaa, eikä esimerkiksi hallinnoi palveluja.   





### 4.4 Oma playbook



Valitsimme vaihtoehto a:n Web-palvelin (web1). Koodi ja suorituksen tulosteet lisättiin Git:iin nimellä ansible\_playbook. 

Kun ajoimme playbookin, kohtasimme virheen. Etsimme apua ensin internetistä ja kokeilimme muokkauksia, mutta päädyimme lopulta käyttämään tekoälyä pienissä määrin. Tästä tarkempaa tietoa raportin lopussa. 





### 4.5 Järjestelmätiedot



|Nimi|Käyttöjärjestelmä|IP-osoite|Prosessorien määrä|Muistin määrä|
|-|-|-|-|-|
|db1|Ubuntu 24.04|10.10.20.102 |12|7538|
|web1|Ubuntu 24.04|10.10.20.101|12|7538|
|client1|Ubuntu 24.04|10.10.10.101|12|7538|
|branch-client|Ubuntu 24.04|10.10.30.101|12|7538|
|attacker|Kali Rolling 2026.3|10.10.10.200|12|7538|
|r1|Ubuntu 24.04|Ei ole|12|7538|
|r2|Ubuntu 24.04|Ei ole|12|7538|
|r3|Ubuntu 24.04|Ei ole|12|7538|







### 4.6 Vertailu



Käsin oli todella tuskaisaa ja hidasta. Pienetkin virheet vaikuttivat heti ja silmät alkoivat väsyä rivejä tuijotellessa. Toisaalta vaikka hidasta ja vaikeaa, tietää ja näkee selvästi mitä tekee. Välillä automaatio menee todella monimutkaiseksi ja ei ymmärrä mitä ihmettä tapahtuu. Toisaalta automaatio on tottakai nopea ja helppo. Automaatio on myös paljon luotettavampi ihmiseen verrattuna. Toisaalta alkuun pääseminen automaatiossa on todella työlästä ja vie hirveästi aikaa, eikä virheitä saisi olla. Kuitenkin alkuun pääsemisen jälkeen työ nopeutuu huomattavasti ja tässä kohtaa ihminen jää jälkeen.  





### 4.7 Pohdinta ja yhteenveto



Automaatio tuo infrastruktuurin ylläpitoon paljon etuja, kuten:



Toistettavuus (sama koodimääritys tuottaa aina täysin samanlaisen lopputuloksen, joten jos palvelin rikkoutuu, uusi samanlainen voidaan pystyttää automaattisesti hetkessä).



Dokumentaatio (selkeät määritystiedot toimivat sellaisenaan ajantasaisena dokumentaationa siitä, miten järjestelmä on rakennettu ja mitä palveluita se sisältää).



Muutosten hallinta (koska määritykset ovat koodia, ne tallennetaan versionhallintaan esim. Git. Kaikista muutoksista jää aukoton historia ja tarvittaessa voidaan palata aikaisempaan toimivaan versioon).



Skaalautuvuus (sama automaatio voidaan ajaa yhtä lailla yhdelle kuin tuhannelle palvelimelle ilman, että ylläpitäjän työmäärä kasvaa samassa suhteessa).



Manuaalinen ylläpito toimii pienissä ympäristöissä, mutta isoissa ympäristöissä automaatio on välttämätöntä järjestelmien toimivuuden kannalta. Esimerkiksi nykyaikaisessa ohjelmistokehityksessä päivityksiä ja koodia julkaistaan jatkuvasti, joten ilman automaattista testausta, paketointia ja jakelua tämä prosessi olisi hidas pullonkaula.



Opimme playbookin käyttöä ja tekemistä, sekä siihen liittyviä komentoja. Opimme myös tutkimaan ansiblen avulla inventoryä ja sen sisältöä. Erilaisia automaation hyötyjä, sekä miten automaatiota tehdään. Esimerkiksi juuri se, kuinka paljon automaatio säästää konkreettisesti aikaa ja virheitä. Tässä tehtävässä esimerkiksi omaa playbookkia tekemällä. Opimme myös ylemmän tehtävän taulukon avulla, kuinka pienillä eroilla on suuria vaikutuksia, kuten komennoissa " ansible\_default\_ipv4" ja " ansible\_all\_ipv4\_addresses". Default\_ipv4 näyttää default ip osoitteen, kun taas all\_ipv4\_addresses näyttää kaikki laitteiden ip osoitteet.





### 4.8 Tekoälyn käyttö



Kohdassa 4.4 oma playbook käytimme tekoälyä, sillä koodi ei toiminut täysin ja näytti erroria.

Suorituksen tuloste näytti aluksi tältä:



" TASK \[Varmista nginx on paalla] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* fatal: \[web1]: FAILED! => {"changed": false, "msg": "Service is in unknown state", "status": {}} 



PLAY RECAP \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* web1 : ok=4 changed=3 unreachable=0 failed=1 skipped=0 rescued=0 ignored=0" 



Emme löytäneet netistä tarvittavaa ratkaisua, joten käytimme Geminiä vastauksen etsimiseen. Tekoäly kehotti meitä vaihtamaan "enabled: yes" komennon "use: service" komentoon. Teimme tämän muokkauksen ja koodi saatiin toimimaan. 



### 4.9 Lähteet



[https://github.com/tjarvenpaa/Verkonhallinta/blob/main/docs/theory/Week04-IaC-ja-Ansible.md](https://github.com/tjarvenpaa/Verkonhallinta/blob/main/docs/theory/Week04-IaC-ja-Ansible.md)



[https://stackoverflow.com/questions/46669172/how-to-fix-permission-denied-error-when-trying-to-install-packages-using-ansible](https://stackoverflow.com/questions/46669172/how-to-fix-permission-denied-error-when-trying-to-install-packages-using-ansible)



[https://docs.ansible.com/projects/ansible/latest/playbook\_guide/playbooks\_handlers.html](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html)



[https://www.cyberciti.biz/faq/ansible-apt-update-all-packages-on-ubuntu-debian-linux/](https://www.cyberciti.biz/faq/ansible-apt-update-all-packages-on-ubuntu-debian-linux/)



[https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/setup\_module.html](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/setup_module.html)







