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

#### Oikean kansion valitseminen

- Siirrytään ensin tmp hakemistoon. Voimme varmistaa missä hakemistossa olemme komennolla: ```pwd```
- Tällöin pitäisi näkyä, että olemme kansiorakenteessa alimmaisena, eli hakemistossa /. Jos näin ei ole niin voimme siirtyä alemmas komennolla: ```cd ..```
- Nyt kun listaamme kaikki kansiot niin meidän pitäisi nähdä haluamamme kansio tmp. Kansioiden listaamienn tapahtuu komennolla ```ls```
- Siirrymme tmp kansioon komennolla: ```cd tmp```
- Nyt komennolla ```pwd``` meidän pitäisi nähdä, että olemme kansiossa /tmp.

#### Liikenteen kaappaaminen tcpdump -työkalulla

- Tässä vaiheessa on hyvä hieman miettiä jo etukäteen, että minkälaista liikennettä meinaamme tuottaa kaapattavaksi. Ukko-laskentaklusteria saattaa käyttää useampi käyttäjä samaan aikaan, joten kaappauksen koko saattaa kasvaa nopeastikin suureksi jos jätämme sen pitkäksi aikaa käyntiin ja alamme sitten vasta miettimään miten voimme tuottaa liikennettä.

- Voimme nyt aloittaa liikenteen kaappaamisen tcpdump työkalulla. Tämä tapahtuu komennolla:
```
sudo tcpdump -Z *omakäyttäjätunnus* -w *omakäyttäjätunnus*.pcap &
```
- Tässä komennossa ```sudo``` määrittelee sen, että käytämme tätä työkalua meille annetuilla sudo oikeuksilla. Komento ```tcpdump``` määrittää käytettävän ohjelman. ```-Z``` lippu määrittää, kuka tiedoston on luonut ja kenellä on oikeus sitä käyttää esimerkiksi tiedoston siirtämiseen myöhemmässä vaiheessa. ```-w```'lippu määrittää tiedoston nimen. Lopussa oleva & -merkki mahdollistaa sen, että tämä prosessi jää tallentamaan liikennettä taustalle ja saamme komentorivin muuhun käyttöön sen ajaksi. Näistä lipuista ja lisäominaisuuksista saat enemmän tietoa komennolla ```man tcpdump```, joka avaa kyseisen työkalun käyttöohjeen komentoriville. & -merkin käytöstä saa vastaavasti lisää tietoa komennolla: ```man bash```

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

#### Tiedoston siirtäminen omalle koneelle 



