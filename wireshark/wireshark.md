## Wireshark

- Tässä tehtävässä on tarkoitus tutustua Wireshark ohjelmiston toiminnallisuuksiin, sekä analysoida haluamaansa aineistoa. Pyrin tässä keskittymään suodattimien sekä datan visualisoinnin opetteluun. Näihin molempiin löytyy Wiresharkista valmiit toiminnallisuudet. Lisäksi tarkoituksena olisi luoda oma suodatin datan käsittelyä varten.

## Alustavat toimenpiteet

### Wireshark asennus

- Wireshark ohjelmiston voi ladata ohjelmiston [viralliselta sivustolta](www.wireshark.org/#download)
- Wireshark oli valmiiksi asennettu käyttämälleni tietojenkäsitelytieteiden laitoksen koneelle.
- Wiresharkin visuaalinen käyttöliittymä käynnistyy helposti asennettuna komentoriviltä komennolla:
```
wireshark
```

### Tutkittava aineisto

- Wireshark tarjoaa [sivustollaan](https://wiki.wireshark.org/SampleCaptures) erittäin kattavasti erilaisia valmiita aineistoja dataa, joita ohjelmistolla voi analysoida. Samalle lataussivustolle pääsee myös ohjelmiston "Help" välilehden kautta kohdasta "Sample Captures".
- Tutkin muutamia erilaisia kiinnostavia aineistoja, kuten esimerkiksi [A trace of ATM Classical IP packets](https://wiki.wireshark.org/uploads/__moin_import__/attachments/SampleCaptures/atm_capture1.cap) nimistä pakettia joka sisälsi pankkiautomaatin lähettämiä IP-paketteja. Paketissa ei kuitenkaan ollut dataa, kuin 12 riviä, joten tätä ei pystynyt käyttämään kovin laajasti visualisointiin.
- Päätin lopulta käyttää kurssin materiaalissa suositeltua [Output from c05-http-reply-r1.jar](https://wiki.wireshark.org/uploads/__moin_import__/attachments/SampleCaptures/c05-http-reply-r1.pcap.gz) nimistä pakettia, joka sisälsikin dataa todella reilusti, yli 68 000 rivin verran.
- Ladatun aineiston pystyy avaamaan ohjelmistossa "File" välilehden kautta kohdasta "Open". Vaihtoehtoisesti myös painamalla Ctrl+O. Lisäksi ohjelman voi käynnistää haluamansa aineiston kanssa komentoriviltä antamalla ohjelman nimen, sekä ladatun paketin nimen. Tässä esimerkissä komennolla:
```
wireshark c05-http-reply-r1.pcap.gz
```

## Suodattimet

- Tässä tehtävässä on tarkoitus tutustua suodattimien käyttöön ja luoda oma suodatin.

### Valmiit suodattimet

- Valmiisiin suodattimiin pääsee tutustumaan välilehden "Analyze" kohdasta "Display filters". Tätä kautta voi myös luoda oman suodattimen ja tallentaa sen ohjelmistoon.
- Suodattimia voi myös käyttää suoraan ohjelmiston päänäkymän "Apply a display filter" -kohdasta. Tämä näkymä näyttääkin kätevästi suoraan monenlaisia esimerkkejä. Esimerkiksi kirjoittamalla "ip" ohjelmisto tarjoaa erilaisia vaihtoehtoja datan suodattamiseen.

### Suodattimien tarkentaminen ja niiden lisäys

- Yksi vaihtoehto on suodattaa dataa siten, että näkyviin jää vain esimerkiksi tietystä IP-osoitteesta (source) tullut data. Tämän avulla on helppo löytää esimerkiksi laajemmasta kokonaisuudesta tietty laite, jonka lähettämää ja vastaanottamaa dataa halutaan analysoida. Tästä testiaineistosta voimme suodattaa datan siten, että näemme vain toisen laitteen lähettämän datan esimerkiksi suodattimella:
```
ip.src == 192.168.0.2
```

- Samaan tapaan myös voimme suodattaa dataa jos haluamme analysoida vain tiettyyn IP-osoitteeseen lähetettyjä (destination) paketteja esimerkiksi jokin tietty internetsivun ip-osoite. Tästä suodattimesta on hyötyä jos esimerkiksi tämän kurssin tutkimuksessa yritämme analysoida jonkin samassa verkossa olevan laitteen lähettämää ja vastaanottamaa dataa. Tästä aineistosta voimme suodattaa vain toiselle laitteelle saapuvan datan esimerkiksi suodattimella:
```
ip.dst == 192.168.0.2
```

- Muita valmiita vaihtoehtoja on esimerkiksi suodattaa pelkät tcp tai upd paketit. TCP pakettien suodatusta voi myös tehostaa siten, että määrittelee vain tietyn portin. Täten voimme suodattaa aineistosta vaikka vain porttiin 80 (HTTP) lähtevät tai sieltä saapuvat paketit.
```
tcp.port == 80
```

- Lisään valmiiden suodattimien listaan yllä mainitut tämän datapaketin lähettäjän ja vastaanottajan IP-osoitteiden suodattamisen. Harjoituksen vuoksi käytän niissä samaa IP-osoitetta, mutta suodatan dataa erikseen ip.dst (destination) ja ip.src (source) komennoilla. Näin saan eroteltua tämä IP-osoitteen lähettämän ja vastaanottamat paketit omiin ryhmiin, jonka avulla voin helpommin visualisoida aineistoa.

## Aineiston visualisointi

- Tässä tehtävässä tarkoituksena on tutustua erilaisiin tapoihin visualisoida dataa.

### I/O graph

- Aineiston saa visualisoitua Wiresharkin välilehden "Statistics" kohdasta "I/O Graph".

- Oletuksena tässä näkymässä on visualisoitu kaikki data jota analysoitava paketti sisältää. Pystyakselille on merkitty pakettien määrä sekunneissa ja vaaka-akselille aika sekunneissa. Voimme myös klikata haluttua kohtaa käyrästä, jolloin wireshark näyttää päänäkymässään sen kohdan jota visualisointi esittää. Tällä tavalla pääsemme tutkimaan tarkemmin grafiikassa näkyviä kiinnostavia kohtia.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/wireshark/All_packets.png" width="1000">

### I/O graph eri suodattimilla

- Voimme nyt tarkastella dataa siten, että käytämme siihen aikaisemmin luomiamme suodattimia. Täten voimme tarkastella kuinka paljon aineistosta löytyvä IP-osoite on vastaanottanut ja lähettänyt paketteja. Alla olevassa visualisoinnissa punainen väri vastaa niiden pakettien määrää, joita IP-osoite 192.168.0.2 on lähettänyt (source) ja sininen väri vsataa pakettien määrää jota kyseinen IP-osoite on vastaanottanut (destination)

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/wireshark/Filtered_IO_graph.png" width="1000">

- Tässä näemme, että suodattimessa käytetty IP-osoite on lähettänyt enemmän paketteja, kuin mitä se on vastaanottanut. Voimme myös tarkastella aineistoa lähempää, jolloin näemme helposti, miten käyrät seuraavat toisiansa. Lähetettyjen pakettien prosentuaalinen osuus näyttäisi olevan silmämääräisesti myös melko paljon suurempi, kuin vastaanotettujen pakettien määrä.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/wireshark/Filtered_IO_graph_zoom.png" width="1000">

### Flow graph

- Flow graph näkymään pääsemme Wiresharkin välilehdeltä "Statistics" kohdasta "Flow graph" 
- Tässä näkymässä voimme tarkastella paljon tietoliikenteen perusteet kurssilla opittuja asioita. Jos vaihdamme näkymän "Flow type" alapalkista muotoon "TCP Flows" voimme tarkastella kuinka TCP yhteys muodostetaan SYN, SYN-ACK ja ACK viesteillä. 
- Näemme myös kohdassa "Comment" lähetettyjen segmenttien (seq=) numerointia ja vastaanottajan kuittauksia saapuneista paketeista (ack=)
- Näemme myös kuinka yhteys puretaan lopuksi FIN ja ACK viesteillä.
- Eri yhteydet on todella kätevästi eroteltuna valmiiksi eri väreillä joten niitä on helppo seurata.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/wireshark/flow_graph.png" width="1000">

### HTTP - Packet counter

- HTTP - Packet counter näkymään pääsemme Wiresharkin "Statistics" kohdsata "HTTP" ja alavalikosta "Packet counter"
- Tässä näkymässä voimme tarkistella esimerkiksi ip.dst == 192.168.0.1 suodattimen avulla, miten tähänä IP-osoitteeseen saapuneet paketit ovat jakautuneet onnistuneisiin ja epäonnistuneisiin. Myös prosentuaalisesti tarkasteltuna mielestäni yllättävän moni paketti on jollain tavoin epäonnistunut.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/wireshark/Packet_counter_dst.png" width="1000">

### Source and Destination Addresses

- Sourse and Destination Addresses näkymään pääsemme Wiresharkin välilehdeltä "Statistics" kohdasta "IPv4 Statisctics" ja alavalikosta "Source and Destination Addresses".
- Tämä näkymä näyttää paljon selkeämmin paketin IP-osoitteiden osuudet siitä, kuinka monta pakettia kukakin on lähettänyt ja vastaanottanut. Todella hyvä työkalu, joka ei tarvitse tämän asian selvittämiseen ylimääräisiä suodattimia.

<img src="https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/kuvat/wireshark/SADA.png" width="1000">


## Mitä opin -osio

- Ilman aikaisempaa kokemusta tästä ohjelmistosta sanoisin, että pääsin sen käyttämiseen mukaan pintapuolisesti melko helposti. Internet on pullollaan hyviä ohjeita tämän ohjelmiston käyttöön, sekä mielenkiintoisia valmiita datapaketteja löytyi paljon. Toiminnoissa ja visualisoinnissa on paljon tietoliikenteen perusteet kurssilla opittuja asioita ja tässä onkin mahdollisuus yhdistää asiat isommaksi kokonaisuudeksi. Ohjelma vaikuttaa todella monipuoliselta ja uskonkin, että perusteiden oppiminen tällä kurssilla antaa avaimet siihen, että ohjelmistoa on helpompi käyttää myös tulevaisuudessa.

## Työaikakirjanpito

[Tästä pääset työaikakirjanpitoon](https://github.com/hhuuskon/tietoliikenne-labra/blob/main/dokumentaatio/tuntikirjanpito.md)