# h4 Omat komennot
Tehtävät ovat osa Tero Karvisen Palvelinten hallinta (Configuration Management Systems)- kurssia.
Kurssitoteutus, tarkemmat tehtäväkuvaukset sekä tehtävän lähteet löytyvät [**täältä**](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/)

Tehtävät on toteutettu Virtual Boxiin (Versio 6.1) asennetulla Ubuntun käyttöjärjestelmällä (versio 22.04.1).
Tehtävät on tehty 22.11.2022 ja tehtävän lähteinä on käytetty luennolla hyödynnettyjä muistiinpanoja.
Luennon piti Tero Karvinen (17.11.2022) ja se oli kurssin neljäs luento, jonka aiheena oli "Omat komennot".

Tehtäväraportti on kirjoitettu markdownilla Ubuntussa ja siirretty sieltä GitHubiin hyödyntäen SSH-yhteyttä.


## a) Hei komento! Tee järjestelmään uusi "hei maailma" -komento ja asenna se orjille Saltilla. Liitä raporttiisi orjan 'ls- l/urs/local/bin/' tulosteesta ainakin se rivi, jolla näkyy uuden komentotiedostosi oikeudet.
Aloitin luomalla kansion scripteille:

    mkdir saltscripts
    
Kokeilin luoda tänne hei maailma- tiedoston helloworld.sls kaikista yksinkertaisimmillaan ensin, jotta voisin sitten testata hiljalleen, että kaikki toimii.

<img width="272" alt="2" src="https://user-images.githubusercontent.com/117899949/203021029-849c8857-6958-4268-acbd-6d8fcbe3f722.png">

Koitin saada komentoa vietyä orjille siinä kuitenkaan onnistumatta. Sain jatkuvasti error-viestin, enkä tiedä, mistä se johtui. Kokeilin myös ajaa pelkkää saltscripts-kansiota, mutta sain samanlaisen errorviestin. En siis saanut tehtävää tehtyä.

<img width="358" alt="3" src="https://user-images.githubusercontent.com/117899949/203021567-9dcfb6a8-9ed5-4186-a329-3da9cbb15934.png">


## b) whatsup.sh. Tee järjestelmään uusi komento, joka kertoo ajankohtaisia tietoja; asenna se orjille.
Loin aiemmin luomaani scripts kansioon sh-tiedoston:

    micro whatsup.sh
    
    #!bin/bash
    echo "Hi, today is:"
    date +%D
    
Tämän jälkeen kokeilin, toimiiko scripti ajamalla sen

    bash whatsup.sh
    
Tuloste oli ok.
Muutetaan tiedoston oikeuksia, jotta saadaan scripti kaikille ajettavaksi

    chmod ugo+x whatsup.sh

Tällä saadaan tiedostolle -rwxr-x-r-x -oikeudet

Kopioidaan/asetetaan skripti muille käyttäjille biniin

    sudo cp whatsup.sh /usr/local/bin
    
Kokeillaan, toimiiko skripti myös muissa hakemistoissa pelkällä tiedoston nimellä. Menin kotihakemistoon ja ajoin koodin:

    whatsup.sh

<img width="362" alt="b2" src="https://user-images.githubusercontent.com/117899949/203022705-445812be-051f-4175-87c9-88036f6c325d.png">

Toimi, kaikki ok!    

## c) hello.py. Tee järjestelmään uusi komento Pythonilla ja asenna se orjille. Shebang on '#!/usr/bin/python3'

Mennään jälleen scripts-kansioon.
Avataan micro ja luodaan hello.py- tiedosto. Luodaan yksinkertainen python3 koodi, kuten "hello".
Testataan koodin toimivuutta ajamalla se komennolla python3

    cd scripts/
    micro hello.py
    
        #!/usr/bin/python3
        print("Hello!")
    
    python3 hello.py

Tuloste ok, scripti siis toimii. Tehdään sama, kun edellisessä tehtävässä.
Eli muutetaan oikeidet ja kopsataan hello.py usr/local/biniin ja testataan jossain muussa hakemistossa, toimiiko scripti pelkällä hello.py- komennolla.

    chmod ugo+x hello.py
    sudo cp hello.py /usr/local/bin/
    
Siirryin roottiin kokeilemaan koodin toimivuutta ja kaikki ok.

    <img width="245" alt="c1" src="https://user-images.githubusercontent.com/117899949/203026495-90d516dc-99c6-419c-8da0-ce478b9ccbad.png">


## d) Laiskaa skriptailua. Tee kansio, josta jokainen skripti kopioituu orjille.



## e) Intel. Etsi [kolme loppuprojektia](https://terokarvinen.com/search/?q=palvelinten+hallinta) joltain vanhalta kurssitoteutukselta.
## Kuvaile tiiviisti, viittaa ja linkitä alkuperäiseen raporttiin. 



## f) Lukua, ei luottamusta. Kokeile yhtä kohdassa e-Intel löytämääsi modulia koneella.
##Tämä on infraa koodina, joten luottamusta ei tarvita. Voit lukea koodista, mitä olet ajamassa.
