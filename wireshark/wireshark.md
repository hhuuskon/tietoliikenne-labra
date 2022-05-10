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
- Suodattimia vois myös käyttää suoraan ohjelmiston päänäkymän "Apply a display filter" -kohdasta. Tämä näkymä näyttääkin kätevästi suoraan monenlaisia esimerkkejä. Esimerkiksi kirjoittamalla "ip" ohjelmisto tarjoaa erilaisia vaihtoehtoja datan suodattamiseen.

### Suodattimien tarkentaminen ja niiden lisäys

- Yksi vaihtoehto on suodattaa dataa siten, että näkyviin jää vain esimerkiksi tietystä ip osoitteesta (source) tullut data. Tämän avulla on helppo löytää esimerkiksi laajemmasta kokonaisuudesta tietty laite, jonka lähettämää ja vastaanottamaa dataa halutaan analysoida. Tästä testiaineistosta voimme suodattaa datan siten, että näemme vain jommna kumman laitteen lähettämän datan esimerkiksi suodattimella:
```
ip.src == 192.168.0.1
```

- Samaan tapaan myös voimme suodattaa dataa jos haluamme analysoida vain tiettyyn ip-osoitteeseen lähetettyjä (destination) paketteja esimerkiksi jokin tietty internetsivun ip-osoite. Tästä suodattimesta on hyötyä jos esimerkiksi tämän kurssin tutkimuksessa yritämme analysoida jonkin samassa verkossa olevan laitteen lähettämää ja vastaanottamaa dataa. Tästä aineistosta voimme suodattaa vain toiselle laitteelle saapuvan datan esimerkiksi suodattimella:
```
ip.dst == 192.168.0.1
```

- Muita valmiita vaihtoehtoja on esimerkiksi suodattaa pelkät tcp tai upd paketit. Tcp pakettien suodatusta voi myös tehostaa siten, että määrittelee vain tietyn portin. Täten voimme suodattaa aineistosta vaikka vain porttiin 80 (http) lähtevät tai sieltä saapuvat paketit.
```
tcp.port == 80
```

- Lisään valmiiden suodattimien listaan yllä mainitut tämän datapaketin lähettäjän ja vastaanottajan ip osoitteiden suodattamisen. Harjoituksen vuoksi käytän niissä samaa IP-osoitetta, mutta suodatan dataa erikseen ip.dst (destination) ja ip.src (source) komennoilla. Näin saan eroteltua tämä IP-osoitteen lähettämän ja vastaanottamat paketit omiin ryhmiinsä jonka avulla voin helpommin visualisoida aineistoa.
