# Pikaohje Mitsurunner laitteen rakentamiseen, ohjelmointiin ja testaukseen.
orginal by iro 11.2021 in https://lampopumput.info/foorumi/threads/msz-ln-sulatushuijaus.31223/page-16#post-493664

# Kokoa Mitsurunner laite alla olevien ohjeiden mukaan
* Osaluettelo: https://github.com/VeliML/MitsuRunner/blob/main/README.md
* Kytkentäkaavio: https://github.com/VeliML/MitsuRunner/blob/main/Pictures/WiringDiagram.png


# Asenna ESPHome  
* Vaatimukset: Python, asenna ensin mikäli et ole vielä asentanut https://python.org/
* Vaatimus macos: Xcode, asenna App Storesta mikäli et ole vielä asentanut.
* Avaa "komentokehote"/terminaali ja asenna ESPHome komennolla `pip3 install esphome`
* Lisäohjeita täältä: https://www.esphome.io/guides/getting_started_hassio.html

==> ESPhome asentuu (asennuksessa mahdollisesti tulevista warningeistä ei tarvitse välittää)
Asennuksen jälkeen voit sulkea terminaalin


# Testaa kokoamasi laite hw_check-ohjelmalla 

* Tallenna hw_check.yaml-tiedosto tietokoneellesi
* Muokkaa tiedostoon oman kotiverkkosi WiFi-parametrit sekä OTA password, ja tarkista että lämpöantureiden ja rele-ohjauksen PIN-määrittelyt vastaavat HW-kytkentääsi

* Kytke laite USB-johdolla tietokoneeseesi
* Avaa komentokehote, siirry Runner-kansioon esim. komennolla `cd kansionpolku` ja anna komento `esphome run hw_check.yaml`
Jos käännös menee OK, ohjelma kysyy lataustapaa (USB vai OTI). Valitse USB.
==> koodi siirretään USB:lla laitteelle

!! Jos lataus ei onnistu tarkista että tietokoneellesi asennettu USB-ohjain käyttämällesi ESPille (esim. Wemos D1 voi tarvita CH34x-ajurin)

Tarkasta:
* Rele naksuu (0.5 sek ON 5 sek välein)
* Windows ruudulle tulostuu logi josta näkyy
* Laite on kytkeytynyt kotiverkkoosi
* DS18B20 antureiden ID_tunnukset (kirjaa ylös, tarvitset näitä myöhemmin) ja mitatut lämpötilat
* Testaa myös OTI-lataus antamalla "esphome mitsurunner.yaml run" uudelleen, mutta valitse nyt OTI lataustapa


# Mitsurunner-ohjelmisto

* Hae Mitsurunner-ohjelma osoitteesta https://github.com/VeliML/MitsuRunner, paina "CODE" ja "Download ZIP"
==> Pakattu Mitsurunner-paketti latautuu koneellesi
* Pura paketti aiemmin luomaasi kansioon (esim. Runner)

* Muokkaa platform.yaml-tiedostoa 
** Määrittele wifi-verkkosi asetukset "ssid" ja "password"
** Määrittele Manual_ip asetukset:
*** static_ip: (ip-osoite, jolla laite kytkeytyy kotiverkkoon)
*** gateway: (ip-osoite jolla kotiverkkosi kytkeytyy kotiverkon ulkoverkkoon, yleensä reitittimen/wifi-laatikon ip-osoite)
Näet kotiverkkosi käyttämän Gateway-ip:n Windows-komentokehotteella "ipconfig"
** Määrittele haluamasi OTA-password

** Määrittele DS18B20-antureiden ID:t (jotka kirjasit ylös hw_check-ohjelmaa käyttäessäsi)
** Tarkista/korjaa tarvittaessa lämpöantureiden ja rele-ohjauksen PIN-määrittelyt vastaamaan HW-kytkentääsi

* Komennolla `esphome run mitsurunner.yaml` käännät ja asennat Mitsurunner-koodin laitteesesi

Tarkista:
Ruudulle tulostuu logi josta näkyy
* Mitsurunner on kytkeytynyt kotiverkkoosi
* DS18B20-antureiden lämpötilat


# Setupin tekeminen IoT-GURUuun

Mene osoitteeseen https://iotguru.live/

Luo itsellesi Account,
* tuon alle DEVICE esim. "wemos"
** tuon alle NODE esim. "mitsu"
*** tuon alle tarvittava määrä FIEDejä, esim. "ulko", "kenno", "delta"
Ohjetta päivitetty 2.1.2023
*** Mitsurunner toiminnan seuraamista auttaa jos määrittelet myös
FIELDit "rssi", "outerror", "heatexcherror","state"

FIELD-välilehden help-osiosta kohdasta GENERIC MQTT TOPIC löydät tyyppiä
`"pub/jGwFuf8I***N2hAddcR6w/p5NVA6zTB***hNCQMvoR7A/itRVfK2***O-yHJgNOcR7A/ulko` 
olevat määritteet, jotka voit kopioida platform.yaml-tiedostoon allakuvatusti


## IoT-GURU parametrien tekeminen Mitsurunneriin:

Editoi platform.yaml-tiedostoa:
`mqtt:
broker: 195.201.219.208 <== IoT-GURUn osoite
id: mqtt_client
username: jGwFuf8I4spe***AddcR6w <== kopio IoT_GURU your accout sivulta löytyvä SHORT IDENTIFIER
client_id: p5NVA6zTBfP***CQMvoR7A <== kopio IoT_GURU "wemos"DEVICE/Device Details-sivulta löytyvä DEVICE SHORT IDENTIFIER
password: h06UcwI2-dAp***4ZvVEVA <== kopio IoT_GURU "wemos"DEVICE/Device Details-sivulta löytyvä DEVICE KEY`

Kopio valmiit mqtt-stringit IoT-GURU-sivuilta

`# MQTT topics. These are inside single quotes
topic_heatexchanger: 'kopio IoT_GURU "mitsu"NODE-"kenno"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'
topic_outdoor: 'kopio IoT_GURU "mitsu"NODE-"ulko"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'
topic_delta: 'kopio IoT_GURU "mitsu"NODE-"delta"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'`

Ohjetta päivitetty 2.1.2023
Jos edellä määrittelit IoT_Guruun lisäFIELDit, tee vastaavat lisäykset myös platform.yaml tiedostoon:
`topic_rssi: 'kopio IoT_GURU "mitsu"NODE-"delta"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'
topic_outdoor_errors: 'kopio IoT_GURU "mitsu"NODE-"delta"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'
topic_exchanger_errors: kopio IoT_GURU "mitsu"NODE-"delta"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'
topic_outdoor_errors: 'kopio IoT_GURU "mitsu"NODE-"delta"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän'`
`# MQTT topics. Notice that these are inside both single and double quotes:`
`topic_state: '"kopio IoT_GURU "mitsu"NODE-"delta"-FIELD/Help-välilehdeltä GENERIC MQTT TOPIC string tähän"'`

===> ovat siis muotoa: /uuu/ccc/nnn/field-name/ , jossa uuu = user_id, ccc = client_id, nnn = node_short_identifier
esim:
`topic_heatexcharger: 'pub/jGwFuf8I4spe***AddcR6w/p5NVA6zTBfP***CQMvoR7A/itRVfK2hvdO-yHJgNOcR7A/kenno'
topic_outdoor: 'pub/jGwFuf8I4spe***AddcR6w/p5NVA6zTBfP***CQMvoR7A/itRVfK2hvdO-yHJgNOcR7A/ulko'`

## IoT-GURUn toiminnan testaaminen

==> Käännä ja lataa Mirsurunner-ohjelma laitteellesi
Windows-ruudulle tulevassa logissa pitäisi näkyä 
`Connecting to MQTT
MQTT Connected!`
ja määrittelemäsi "kenno", "ulko" ja "delta" lämpötilat päivittyvät IoT-GURUun

# Ohjetta päivitetty 2.1.2023
Jos edellä määrittelit IoT_Guruun lisäFIELDit
"rssi" = WiFi-signaalin taso
"outdoor_errors" = outdoor-anturin lukuvirheet
"exchanger_errors" = exchanger-anturin lukuvirheet
"state" = Mitsurunner tilakoneen tila
