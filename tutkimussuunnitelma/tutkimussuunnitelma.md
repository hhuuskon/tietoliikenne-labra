## Tutkimussuunnitelma päätelaitteiden lähettämistä WiFi verkkojen kyselyistä.

#### Tutkimuskysymys

Mitä tietoa päätelaite antaa itsestään ulos jos jätämme WiFi -toiminnon päälle lähtiessämme pois kotoa, koulusta tai työpaikaltamme. Alkaako laite siis kyselemään ympäriltään aikaisemmin tallennettuja langattomia verkkoja niiden nimillä (SSID). Voiko tällainen tieto kertoa jotain laitteen omistajasta? Pystyykö tällaisen tallennetun tiedon analysoinnissa lisäksi päättelemään radiosignaalin voimakkuudesta millä etäisyydellä tämä päätelaite on?

#### Hypoteesi

Päätelaitteet todennäköisesti kyselevät näitä tallennettuja verkkoja ja ne saattavat kertoa kantajastaan sen missä he ovat aikaisemmin vierailleet. Jos tallennettu verkko on nimetty verkon ylläpitäjän toimesta kuvaavasti (esim. Sokos Hotelli Helsinki) paljastaa se jo selkeästi jotain. Lisäksi signaalin voimakkuudesta voimme todennäköisesti päätellä kuinka lähellä käyttäjä fyysiesti on tallennuksen tapahtuessa. Tällaista liikennetta todennäköisesti tulee todella paljon, kun tallennus aloitetaan.

#### Tutkimusmenetelmät

Tutkin ympärillä olevaa WiFi liikennettä tietokoneella johon on liitetty erillinen verkkokortti. Tämä verkkokortti tukee tilaa jossa ympärillä olevia langattoman verkon signaaleita pystytään monitoroimaan (monitor mode). Tallennan tätä dataa Wireshark ohjelmistolla ja suodatan sieltä juuri nämä tunnettujen langattomien verkkojen kyselyt (probe) ja siitä erityisesti kiinnostavat verkon nimet (SSID). Tämän jälkeen voin poistaa oman puhelimeni sen tuntemasta verkosta ja katsoa alkaako se kyselemään laitteeseen tallennettuja verkkoja. Tätä tallennusta ei todennäköisesti tarvitse tehdä kovin montaa minuuttia. Jos tutkimus onnistuu niin yritän rakentaa erillisen pienemmän kannettavan laitteen jonka pystyisi myös ottamaan mukaan. Tämän laitteen avulla pystyisin nauhoittamaan pidempiä aikoja näitä verkkojen nimiä. Jos tämä tuottaa tulosta niin datan tarkasteluun voisi ottaa mukaan myös signaalien voimakkuuksien tallentaminen.

#### Aikataulu

Liikenteen kaappaminen ja sopivien suodattimien löytäminen 1h
Datan tarkastelu 2h
Kannettavan laitteen rakentaminen ja toimintaan ottaminen 3h
Liikenteen kaappaminen ja suodattimien lisääminen tarvittaessa 2h
Datan tarkastelu ja tutkimuksen osalta kiinnostavien SSID:iden etsiminen 2h
Posterin tekeminen ja tutkimuksen dokumentointi 8h

Yhteensä 18h

#### Posterin sisältö

Tutkimuskysymys ja sen esittäminen henkilön tietoturvan näkökulmasta
Kaappausten ja laitteiston esittäminen
Kiinnostavan datan esittely ja mitä siitä huomaamme
Yhteenveto ja mitä henkilö voi tehdä parantaakseen tietoturvaansa
Mahdolliset lähteet

#### Työaikakirjanpito

14.05.2022 | Liikenteen tallentamisen kokeilu etukäteen 1h
16.05.2022 | Tutkimussuunnitelman valmistelu 2h
17.05.2022 | Liikenteen tallentamisen toinen kokeilu 1h
18.05.2022 | Tutkimussuunnitelman kirjoittamista 2h

Yhteensä 6h

