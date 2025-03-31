# H1 Viisikko

## Lue ja tiivistä
### Run Salt Command Locally https://terokarvinen.com/2021/salt-run-command-locally/)
-	Salt on etähallintatyökalu, jolla voidaan kontrolloida lukuisia orja tietokoneita
-	Salt toimii Linuxilla, sekä Windowsilla
-	Huom. Muista päivittää ennen asennusta, `sudo apt-get update`

### Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/ 
-	Master tulee olla julkisessa verkossa tai tiedetyssä verkossa, kun taas orjat voivat piileskellä palomuurin takana.
-	Jokaisella orjalla tulee olla erilainen tunnus (ID)
-	Huom. tehtävä A) toteutettu Teron ohjeita noudattaen https://terokarvinen.com/2021/install-debian-on-virtualbox/ , palomuuri on asetettu päälle ja ohjeistuksessa on maininta siitä, että portit tulee avata Masterille, mikäli sillä ei ole oikeuksia

### Raportin kirjoittaminen, https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/ 
-	Viittaa aina, jos olet jostain saanut tiedon tai tulevissa tehtävissä mistä löysit komennon 
-	Raportit saattavat auttaa myös itseäsi, kun kirjoitat niitä samaan aikaan kun teet tehtäviä.
-	Tee myös raportit sen verran selkeästi, että kuka vain ymmärtää ne ja tajuaa mitä on tehty
-	Huom. Älä valehtele, jos et ole oikeasti tehnyt kyseistä elä myöskään plagioi

### Linux (DEB)¶ https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html 
-	Sivustolta löytyy ohjeistus siihen, miten Salt kuuluu asentaa Debian pohjaiselle käyttäen apt-get install 
-	Salt asennus vaatii oman arkiston
-	3006 Long-Term Support on turvallisempi ja vakaampi kuin 3007 Short Term Support, missä on taas uusimmat ominaisuudet heti käytössä.
-	Huom. Kun asennat tietyn onedir-version Saltista se asentaa oman paikallisen Python version myös, järjestelmääsi tulee Python lisäosa.

## Tehtävät

## Asenna Salt (salt-minion) Linuxille (uuteen virtuaalikoneeseesi).
Salt asennus, käytin Salt Project sivuston ohjeita kyseiseen asennukseen https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html 
Tein polun Saltille käyttäen komentoa, mkdir -p /etc/apt/keyrings tämän jälkeen latasin julkisen avaimen Saltille käyttäen curl komentoa.
![1](https://github.com/user-attachments/assets/20352ec1-890c-44d5-b316-ae54c1391ac5)

näiden jälkeen, lisäsin vielä toisen `curl` komennon mikä lataa suoraan uusimman version Saltista, sekä myös päivitys käyttäen komentoa sudo apt update.

![3](https://github.com/user-attachments/assets/2fe2cf2a-fcd9-4998-b96d-d6ce05f2e9f7)

Kyseisten steppien jälkeen latasin Salt minionin, tarkastin samalla kyseisen version.

![4](https://github.com/user-attachments/assets/f56a7673-0a72-4080-b7e2-63517da8de00)
![6](https://github.com/user-attachments/assets/ee2ef589-edb4-4e43-ac9c-64b3c65764fa)

## Viisi tärkeintä, Saltin Tilafunktiot pkg, file, service, user, cmd. 

### PGK

Komennolla,  `sudo salt-call --local -l info state.single pkg.installed tree` , ladataan tree ja saadaan tulostus näkyviin.

![image](https://github.com/user-attachments/assets/526d4d33-30d6-4499-aa26-d51ce1edaf37)

Tulostuksesta selviää, että asennus on onnistunut ja ei ole tullut minkään näköisiä ongelmia latauksen yhteydessä ja asennusprosessi kesti 3.063 sekunttia, selviää myös se, että mikä versio on ladattu kyseisen versio on 2.1.0-1

Kyseinen tree myös poistetaan komennolla `sudo salt-call --local -l info state.single pkg.removed tree`

![image](https://github.com/user-attachments/assets/ff929018-a54d-4353-a823-ac4259a3ba2b)

Tulostuksesta selviää function kohdasta mitä on tehty ja nyt huomasin myös, että kyseinen ei ole enää "new" vaan se on siirtynyt "old" kohtaan, ja edelliseen lataukseen liittyen tämäkin on onnistunut sillä "Result" kohta näyttää true ja alhaalla on tieto siitä, että se on "Succeeded" eikä "Failed"

### FILE

Komennolla `sudo salt-call --local -l info state.single file.managed /tmp/hellotero` tehdään tyhjä tiedosto.

![image](https://github.com/user-attachments/assets/93a9d321-a352-4323-9be1-9f2406ab3d0b)

Kuvasta nähdää kohdassa "INFO" sekä "Changes" että tiedoston luonti on onnistunut ja kommentti kertoo sen, että kyseinen on tyhjä eikä se sisällä mitään

Tämän jälkeen käytin komentoa `sudo salt-call --local -l info state.single file.managed /tmp/moitero contents="foo"`

![image](https://github.com/user-attachments/assets/a7f1420d-17c6-4775-adf1-16c9d18ad2aa)

Edelliseen tulostukseen verrattean "Comment" kohtaan oli ilmestynyt teksti File "****" updated ja "changes" kohtaan oli tullut muutoksia aikaisemmin näkyi "new:" kyseinen kohta oli nyt nimellä "diff" ja ylhäällä "INFO" riville oli myös tullut teksti "File changed"

Osion file viimeinen komento `sudo salt-call --local -l info state.single file.absent /tmp/hellotero` , puolestaan poistaa tiedoston

![image](https://github.com/user-attachments/assets/37d1dbf1-c8f7-441a-9e45-ca5808cd6689)

Tulostuksesta selviää, että tiedosto on onnistuneesti poistettu, sillä failed on 0, "comment" kohdasta näkyi vielä selvästi mitä tehtiin.

### SERVICE
Käynnistin servicen komennolla `sudo salt-call --local -l info state.single service.running apache2 enable=True`

![image](https://github.com/user-attachments/assets/4073c2ac-81f8-4c06-b442-60675286eb87)
Sain ensimmäisen virheilmoituksen tähän mennessä kommentin perusteella kumminkin pystyin näkemään ongelman, sillä tiesin, että apache2 ei ole edes ladattu kyseiseen virtuaalikoneeseen

Virhe korjattu kommennolla `sudo apt update && sudo apt install apache2` tämän jälkeen kokeilin uusiksi komentoa `sudo salt-call --local -l info state.single service.running apache2 enable=True`



![image](https://github.com/user-attachments/assets/02dc3895-cec3-493d-b3a9-90da6fb08c13)

Tulostuksesta selvisi kun ilmoitus ruutu ei näytä paholaisen punaista vaan vihreetä, niin kyseinen service tilafuntio on onnistunut ja "function" kohdasta myös selviää se, että kyseinen on tällä hetkellä toiminnassa.

Seuraava komento minkä annoin on `sudo salt-call --local -l info state.single service.dead apache2 enable=False` kommennossa näkyy jo `service.dead` mikä viittaa siihen, että Apache2 aika on tullut kuolla.

![image](https://github.com/user-attachments/assets/8cc12aae-9833-44ba-ba8e-8eff623c471e)

Tulostuksesta selvisi se, että service apache2 on onnistuneesti kuollut.


### USER
Seuraava komento minkä annoin oli `sudo salt-call --local -l info state.single user.present steffe22`

![image](https://github.com/user-attachments/assets/2a15fa8e-cc32-45cd-96ab-73c9f5e954d8)

Komento loi käyttäjän "steffe22" serverille onnistuneesti, tulostuksesta selvisi, että käyttäjälle oli luotu kotikansio ja muut.

Seuraavksi oli Steffen aika poistua serveriltä niin annoin komennon `sudo salt-call --local -l info state.single user.absent steffe22`

![image](https://github.com/user-attachments/assets/abc19d0c-76d7-4845-853b-d15b70bf58d2)

Tulostuksesta selvisi erittäin nopeasti se, että steffe22 oli poistettu onnistuneesti ja myös kyseiset kotikansiot sun muut on poistettu.


### CMD

Viimeinen komento jota piti syöttää oli `sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo"`

![image](https://github.com/user-attachments/assets/b0bfa0e0-d261-4bad-9c73-22efb672cf51)

Tulostuksesta selviää mitä cmd komento tekee, se suorittaa touch komennon mikä luo tyhjän tiedoston /tmp/foo sijaintiin.

## Idempotentti

Itselle aivan uusi käsite, minkä vuoksi pääsin lukemaan asiasta sivustolla, https://www.freecodecamp.org/news/idempotence-explained/

Idempotentti tarkoittaa sitä, että vaikka kuinka monesti komento lyötäisiin serverille ei se muuta järjestelmän tilaa, yhtä lailla kun bussissa ensimmäisen kerran joku painaa stop nappulaa pysähtyy bussi seuraavalle pysäkille, vaikka kuinka monesti painaisit bussin stop painiketta ei bussi pysähdy seinään niin sanotusti.

käytin komentoa mitä aikaisemmin jo käytin alussa, kun latasin sovelluksen `tree sudo salt-call --local -l info state.single pkg.installed tree`

Ensimmäinen kerta.

![22](https://github.com/user-attachments/assets/948ee4c0-3f9b-44fc-9afe-fcd11550cc60)


Tulostuksesta selviää se, että se on ladattu onnistuneesti.


Toinen kerta kun annoin saman komennon

![44](https://github.com/user-attachments/assets/f9e40377-57c8-4084-be9b-3972f8dc7242)


Nyt tulee itse Idempotentti esiin, tulostuksessa näkyy, että kyseinen on jo asennettu. Vaikka annoin komennon, että lataa sovellus tree, se ei sitä suorita koska se tietää, että kyseinen on jo ladattu ja tästä syystä ilmoittaa vaan asiasta, että se on ladattu valmiiksi jo.






Lähteet 




Reintech 2024. Installing Apache on Debian 12: A Step-by-Step Guide https://reintech.io/blog/installing-apache-on-debian-12-step-by-step-guide 


