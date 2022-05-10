## Wireshark

- Tässä tehtävässä on tarkoitus tutustua Wireshark ohjelmiston toiminnallisuuksiin, sekä analysoida haluamaansa aineistoa. Pyrin tässä keskittymään suodattimien sekä datan visualisoinnin opetteluun. Näihin molempiin löytyy Wiresharkista valmiit toiminnallisuudet. Lisäksi tarkoituksena olisi luoda oma suodatin datan käsittelyä varten.

### Wireshark asennus

- Wireshark ohjelmiston voi ladata ohjelmiston [viralliselta sivustolta](www.wireshark.org/#download)
- Wireshark oli valmiiksi asennettu käyttämälleni tietojenkäsitelytieteiden laitoksen koneelle.
- Wireshark käynnistyy helposti asennettuna komentoriviltä komennolla:
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

