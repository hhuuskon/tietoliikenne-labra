## Liikenteen tallennus virtuaalikoneella

- Tämän tehtävän tarkoituksena oli harjoitella tietoliikenteen tallennusta ja analysoida siitä saatua dataa. Liikenne tallennetaan ukko-laskentaklusterilla, jolla saattaa olla myös muita samanaikaisia käyttäjiä. Tuotamme tuolla virtuaalikoneella tallennuksen aikana itse liikennettä, jonka voimme erotella muusta datasta myöhemmin. Tämän jälkeen siirrämme tallennetun datan virtuaalikoneelta omalle koneellemme ja analysoimme siitä oman tuottamamme liikenteen Wireshark ohjelmistolla.

### Yhteyden muodostaminen ukko-laskentaklusterille

- Emme voi suodaan omalta koneeltamme ottaa yhteyttä ukko-laskentaklusteriin, joten meidän täytyy ensin ottaa SSH yhtyes laitoksen virtuaalikoneelle. Käytän tässä esimerkissä itselleni tutuksi tullutta melkki.cs.helsinki.fi virtuaalikonetta. Tämä tapahtuu komentorivin komennolla:
```
ssh *omakäyttäjätunnus*@melkki.cs.helsinki.fi
```

- Tämän jälkeen komentorivi kysyy sinulta salasanaa. Salasana on sama, kuin muihinkin yliopiston palveluihin käyttämäsi salasana.

- Jos kirjautumistietosi olivat oikein niin näet nyt komentorivillä, että tietokoneesi nimi on muuttunut muotoon *omakäyttäjätunnus*@melkki.

- Tämän jälkeen pääsemme ottamaan yhteyden ukko-laskentaklusteriin. Tämä tapahtuu samalla tavalla, kuin yhteydenotto melkki virtuaalikoneeseen, eli SSH yhteyden avulla. Käytämme tähän komentoa:
```
ssh *omakäyttäjätunnus*@svm-11.cs.helsinki.fi
```
- Jos sinulla on tarvittavat oikeudet niin näet nyt komentorivillä, että tietokoneesi nimi on taas muuttunut muotoon *omakäyttäjätunnus*@svm-11. Yhteys ukko-klusteriin on nyt muodostettu.

### liikenteen tallennus virtuaalikoneella

- Voimme nyt siirtyä seuraavaan vaiheeseen, jossa tallennamme virtuaalikoneen verkkoliikennettä tcpdump työkalulla.

### Oikean kansion valitseminen

- Tämä tuntuu ehkä hieman oudolta lisäykseltä dokumentointiin, mutta oli erittäin tärkeässä roolissa tässä tehtävässä. Tämä johtuu siitä, koska virtuaalikoneen käyttöoikeudet ovat rajattuja.
- Siirrytään ensin tmp hakemistoon. Voimme varmistaa missä hakemistossa olemme komennolla: ```pwd```
- Tällöin pitäisi näkyä, että olemme kansiorakenteessa alimmaisena, eli hakemistossa /. Jos näin ei ole niin voimme siirtyä alemmas komennolla: ```cd ..```
- Nyt kun listaamme kaikki kansiot niin meidän pitäisi nähdä haluamamme kansio tmp. Kansioiden listaamienn tapahtuu komennolla ```ls```
- Siirrymme tmp kansioon komennolla: ```cd tmp```
- Nyt komennolla ```pwd``` meidän pitäisi nähdä, että olemme kansiossa /tmp.

### Liikenteen kaappaaminen tcpdump -työkalulla

- Tässä vaiheessa on hyvä hieman miettiä jo etukäteen, että minkälaista liikennettä meinaamme tuottaa kaapattavaksi. Ukko-laskentaklusteria saattaa käyttää useampi käyttäjä samaan aikaan, joten kaappauksen koko saattaa kasvaa nopeastikin suureksi jos jätämme sen pitkäksi aikaa käyntiin ja alamme sitten vasta miettimään miten voimme tuottaa liikennettä.

- Voimme nyt aloittaa liikenteen kaappaamisen tcpdump työkalulla. Tämä tapahtuu komennolla:
```
sudo tcpdump -Z *omakäyttäjätunnus* -w *omakäyttäjätunnus*.pcap &
```
- Tässä komennossa ```sudo``` määrittelee sen, että käytämme tätä työkalua meille annetuilla sudo oikeuksilla. Komento ```tcpdump``` määrittää käytettävän ohjelman. ```-Z``` lippu määrittää, kuka tiedoston on luonut ja kenellä on oikeus sitä käyttää esimerkiksi tiedoston siirtämiseen myöhemmässä vaiheessa. ```-w``` lippu määrittää tiedoston nimen. Lopussa oleva & -merkki mahdollistaa sen, että tämä prosessi jää tallentamaan liikennettä taustalle ja saamme komentorivin muuhun käyttöön sen ajaksi. Näistä lipuista ja lisäominaisuuksista saat enemmän tietoa komennolla ```man tcpdump```, joka avaa kyseisen työkalun käyttöohjeen komentoriville. & -merkin käytöstä saa vastaavasti lisää tietoa komennolla: ```man bash```

- Nyt voimme tuottaa jotain liikennettä jonka saamme mukaan tallennukseen. Itse valitsin tässä tehtävässä käyttää komentoa:
```
ping helsinki.fi
```
- Otin talteen syötteet, jotten niiden löytäminen olisi helpompaa myöhemmässä vaiheessa kaiken muun liikenteen joukosta. Syöte näytti seuraavanlaiselta.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/tietoliikenne/ping_helsinki.png" width="1000">

- Voimme sammuttaa ```ping``` työkalun painamalla ctrl + z, kun olemme mielestämme tuottanut riittävän määrän liikennettä.
- Voimme sammutta tcpdump työkalun palaamalla siihen komentorivillä komennolla: ```fg``` ja painamalla ctrl + z.
- Jos tiedoston sammuttaminen ei onnistu niin voit komennolla ```jobs``` listata käynnissä olevat työt. Tämän jälkeen näet listan käynnissä olevista (Running) ja keskeytetyistä (Stopped) töistä. Tästä näet myös työn etupuolelta työn numeron jota käyttämällä voit tuoda työn eteen käyttäen työn numeroa komennolla ```fg %*työn numero*. Tämän jälkeen kokeile uudelleen sammuttaa työ painamalla ctrl + z.
- Tämän jälkeen jos käytämme komentoa ```ls -la``` meidän pitäisi nähdä luomamme tiedosto, sekä se, että olemme tiedoston omistaja. En tähän valitettavasti voi lisätä havainnollistavaa kuvaa, sillä siinä ei näy muuta, kuin käyttäjätunnukset.

### Tiedoston siirtäminen omalle koneelle 

- Siirrämme ensin ukko-laskentaklusterilta tämän juuri luomamme .pcap tiedoston aikaisemmin käyttämällemme melkki.cs.helsinki.fi virtuaalikoneellemme. Tämä voidaan toteuttaa SCP työkalulla. Käytämme tiedon siirtämisessä komentoa:
```
scp *tiedostonnimi* *omakäyttäjätunnus*@melkki.cs.helsinki.fi:~/
```
- Tässä komennossa ```scp``` määrittää käytettävän ohjelman. Tämän jälkeen määrittelemme tiedoston .pcap jonka haluamme siirtää. Sen jälkeen määritämme virtuaalikoneen jolle haluamme tiedoston siirtää. Lopussa oleva : merkin jälkeen määritämme kansion johon haluamme tiedoston siirtää. Tässä tapauksessa siirrämme tiedoston omaan kotihakemistoomme käyttämällä tuota ```~/``` päätettä.
- Nyt voimme poistua ukko-laskentaklusterilla yksinkertaisesti komennolla: ```exit```.
- Nyt olemme palanneet melkki.cs.helsinki.fi virtuaalikoneelle jossa näemme ```ls``` komennolla juuri siirtämämme tiedoston.
- Voimme vaihtoehtoisesti nyt avata koneellamme uuden komentorivin jolloin pääsemme käyttämään komentoja omalla paikallisella tietokoneellamme. Voimme myös poistua melkki.cs.helsinki.fi virtuaalikoneelta komennolla ```exit```.
- Syötämme omalla koneellamme paikallisesti komennon:
```
scp *omakäyttäjätunnus*@melkki.cs.helsinki.fi:*tiedostonnimi* *polku johon tiedoston haluamme*
```
- Tämän jälkeen tiedosto pitäisi löytyä määrittelemästämme polun sisältä.

### Liikenteen analysointi


Kun olemme tiedoston kanssa samassa hakemistossa voimme ladata juuri siirtämämme tiedoston wireshark ohjelmaan komennolla:
```
wireshark *tiedostonnimi*
```
- Voimmekin heti suodattaa nyt helposti viime tehtävässä oppimallamme tavalla ja aikaisemmin talteenotetulla IP-osoitteella vain ne paketit jotka itse teimme käyttämällä ```ping``` komentoa.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/tietoliikenne/wireshark_filtered.png" width="1000">

- Näemme yllä olevan kuvan alakulmasta, että olemme onnistuneet suodattamaan 1220 paketin joukosta ne 98 pakettia jotka itse teimme.
- Tässä näkyy myös hyvin, että joka toinen paketti on ping pyyntö (request) ukko-laskentaklusterilta helsinki.fi IP-osoitteeseen ja joka toinen paketti on vastaus (reply) helsinki.fi IP-osoitteelta ukko-laskentaklusterille.
- Näemme myös, että pakettien koko on 98 Tavua (784 bittiä). Katsoessa muista paketteja ne näyttävät kaikki olevan saman kokoisia.


<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/tietoliikenne/traffic_graph_1.png" width="1000">

- Yllä olevasta kuvasta näemme I/O Graph visualisoijan avulla, että tuottamamme liikenne (vihreällä) kaikesta pakettiin eksyneestä liikenteestä (mustalla) on todella vähäistä. Tämä on hyvä tiedostaa jatkossa, ettei lähellekkään kaikki kaapattu liikenne ole välttämättä sitä, mitä itse on tarkoittanut tutkittavaksi tuottaa. Tämän vuoksi suodattimien käyttö jo kaappaus vaiheessa voisi olla järkevää.


<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/tietoliikenne/flow_graph_1.png" width="1000">

- Yllä olevasta kuvasta huomaamme myös, että emme aloitimme liikenteen tallentamisen ensin, sillä tekemämme ```ping``` komennot näkyvät vasta ajankohdassa n.23. 

## Mitä opin -osio

- Tämä tehtävä oli opettavainen monellakin tapaa. SCP ja Tcpdump olivat minulle kokonaan ennestään tuntemattomia ohjelmia. Niiden käyttö oli kuitenkin melko selkeää, kun kaikkien muidenkin ohjelmien tavoin jaksoi lukea niiden manuaalit. Oli todella mielenkiintoista päästä käyttämään ukko-laskentaklusteria. Itselläni suurin ongelma oli tiedoston siirrossa virtuaalikoneelta omalle koneelle. Tähän onneksi löytyi hyvä apu ohjauksesta. Lisäksi Wiresharkin käyttö tässäkin tehtävässä tuntui nyt helpommalta, kuin sitä oltiin jo edellisessä tehtävässä jonkin aikaa pyöritelty. Graafien käyttöön toivoisin silti oppivani vielä jotain monipuolisempaa käyttöä.

## Työaikakirjanpito

[Tästä pääset työaikakirjanpitoon](https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/tuntikirjanpito.md)




