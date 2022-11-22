# h4 Omat komennot
Tehtävät ovat osa Tero Karvisen Palvelinten hallinta (Configuration Management Systems)- kurssia.
Kurssitoteutus, tarkemmat tehtäväkuvaukset sekä tehtävän lähteet löytyvät [**täältä**](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/)

Tehtävät on toteutettu Virtual Boxiin (Versio 6.1) asennetulla Ubuntun käyttöjärjestelmällä (versio 22.04.1).
Tehtävät on tehty marraskuussa 2022 ja tehtävän lähteinä on käytetty luennolla hyödynnettyjä muistiinpanoja.
Luennon piti Tero Karvinen (17.11.2022) ja se oli kurssin neljäs luento, jonka aiheena oli "Omat komennot".

Tehtäväraportti on kirjoitettu markdownilla Ubuntussa ja siirretty sieltä GitHubiin hyödyntäen SSH-yhteyttä.


## a) Hei komento! Tee järjestelmään uusi "hei maailma" -komento ja asenna se orjille Saltilla. Liitä raporttiisi orjan 'ls- l/urs/local/bin/' tulosteesta ainakin se rivi, jolla näkyy uuden komentotiedostosi oikeudet.

En ole täysin varma, ymmärsinkö tehtävänantoa oikein, mutta lähdin tässä luomaan txt-tiedostoa, jonka ajoin saltilla orjille.

Tehtävän kulku:
Ensin tein päivitykset.

    sudo apt-get update
    sudo apt-get upgrade

Tarkistin, että Salt löytyy koneeltani. Komento "Salt" varmisti, että kaikki ok.
Sitten siirryin salt-hakemistoon

    cd /srv/salt

Loin heimaailma-tekstitiedoston heimaailma.txt simppelillä sisällöllä "Hei Maailma!".
Tämän jälkeen muutin oikeudet ja kopsasin sen biniin:
    
    sudo chmod ugo+x heimaailma.txt 
    sudo cp heimaailma.txt /usr/local/bin/

Tämän jälkeen loin salt state-tiedoston:
    
    sudo micro heimaailma.sls
    
<img width="267" alt="image" src="https://user-images.githubusercontent.com/117899949/203038122-2a5485e1-7990-4f5b-b152-9e08568b7380.png">

Ajetaan heimaailma orjille:

    sudo salt-call --local state.apply heimaailma

Tämä näytti toimivan.

<img width="365" alt="image" src="https://user-images.githubusercontent.com/117899949/203038720-763dd2e2-dea5-4055-b57d-3951aa2446c3.png">

Mennään kurkkaamaan usr/local/biniin heimaailma tiedoston oikeudet.
Oikeudet ovat ne, jotka aiemmin määriteltiin, eli ownerille rwx, groupille ja othersille read ja execute.

<img width="341" alt="image" src="https://user-images.githubusercontent.com/117899949/203039408-9647deb7-0baa-436c-b952-69b432b17144.png">


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

Tuloste ok, scripti siis toimii työhakemistossa. Tehdään sama, kun edellisessä tehtävässä, jotta saadaan scripti toimimaan kaikkialla.
Eli muutetaan oikeidet ja kopsataan hello.py usr/local/biniin ja testataan jossain muussa hakemistossa, toimiiko scripti pelkällä hello.py- komennolla.

    chmod ugo+x hello.py
    sudo cp hello.py /usr/local/bin/
    
Siirryin roottiin kokeilemaan koodin toimivuutta ja kaikki ok.

<img width="245" alt="c1" src="https://user-images.githubusercontent.com/117899949/203026495-90d516dc-99c6-419c-8da0-ce478b9ccbad.png">


## d) Laiskaa skriptailua. Tee kansio, josta jokainen skripti kopioituu orjille.
Aloitin tehtävän a-luomalla skritpikansion, joten hyödynnetään tätä.

Aloitin luomalla kansion scripteille:

    cd /srv/salt
    mkdir saltscripts

Loin tässä kohtaa kolme erilaista bash-script tiedostoa:
whatsup.sh, hello.sh & moi.sh

Testasin, että scriptit toimii paikallisesti.

<img width="331" alt="dtest" src="https://user-images.githubusercontent.com/117899949/203033987-118ecbaa-4ca7-4ed9-b7c8-4c5c7916154d.png">

Tässä kohtaa ei muuteta oikeuksia, sillä ne voidaan määrittää state.tiedostoon, jonka loin samaan saltscriptit kansioon nimellä: init.sls

State-tiedostoon lisätään kaikki aiemmin kolme luomaami scripti-tiedostoa jokainen omana kappaleenaan.
Sourceen määritellään salt-polku tiedostoille ja modeen oikeudet.
    
    /usr/local/bin/<tiedostonnimi>:
      file.managed:
        - source: salt://<kansionnimi>/<tiedostonnimi>
        - mode: wrx-oikeuksien määrittely


<img width="360" alt="d2" src="https://user-images.githubusercontent.com/117899949/203032397-dd4e400d-dbfb-4a62-884f-ac1b91294d9a.png">

Tämän jälkeen kokeilin ajaa ko. kansion orjille salt-komennolla:
    
    sudo salt '*' state.apply saltscripts
    
<img width="366" alt="image" src="https://user-images.githubusercontent.com/117899949/203034361-75d149ad-c9b1-4a24-addc-afa28eb6c556.png">
    
Resultiksi saadaan "True" kaikkien kolmen scriptin osalta, mutta nyt pitäisi vielä testata, että toimiiko ko. scriptit orjilla.
Mennään roottiin ja ajetaan 
    
    moi.sh

<img width="196" alt="image" src="https://user-images.githubusercontent.com/117899949/203035230-4ce60210-3a1c-4873-a99e-f4fae7b4a67d.png">

Näyttäisi toimivan.

## e) Intel. Etsi [kolme loppuprojektia](https://terokarvinen.com/search/?q=palvelinten+hallinta) joltain vanhalta kurssitoteutukselta. Kuvaile tiiviisti, viittaa ja linkitä alkuperäiseen raporttiin. 

**Projekti 1: Joonas Kulmala, Salt My Ubuntu**
[Tässä](https://github.com/JoonasKulmala/Palvelinten-Hallinta/tree/main/h7) raportissa kuvaillaan automatisointiprosessia, jolla saadaan asennettua Salt Linux Ubuntulle (versio 20.14).
Raportin idea on kloonata ko. repository (tai downloadata ZIP), modata minion-filea, ajaa bashskripti sekä confata Salt-master ja/tai Minion.
Projektissa on kuvattu selkeät ohjeet step by step-tyyliin ja lopuksi vielä testataan ratkaisua.

**Projekti 2: Jani Aulavuo Palvelinten hallinta: Salt LAMP Stack**
[Tässä](https://bgm064.wordpress.com/2021/04/06/palvelinten-hallinta-2021-h1/8/)raportissa luotiin salt-tila, joka asentaa LAMP Stack- ohjelmointipinon, UFW-palomuurin sekä Firefox-selaimen.
Raportin alussa on kuvattu alkuun hieman teoriaa siitä, mistä LAMP Stack koostuu (Linux, Apache, MySQL, PHP), jonka jälkeen lähdetään kuvaamaan työosuutta.
Raportti on enemmän kuvaileva, kun ohjeistava, vaikka se sisältääkin ohjeistuksen ko. tehtävään.

**Projekti 3: Steam, pelikauppa, jossa paljon digitaalisia rajoitusmenetelmiä (DRM)**
[Tässä](https://myllys.wordpress.com/palvelinten-hallinta-harjoitus-7-oma-moduli-spring-2021/) projektissa kuvaillaan sellaisen Salt-tilan tekemistä, joka asentaa automaattisesti steamin uudelle Ubuntu-koneelle.
Raportissa ei kuvailla, mikä on steam, jolloin se jää asiasta tietämättömälle täysin mielikuvituksen varaan.
Tämä raportti on ennemmän kuvaileva, kun ohjeistava, vaikkakin jokainen askel kuvaillaan yksityiskohtaisesti.


## f) Lukua, ei luottamusta. Kokeile yhtä kohdassa e-Intel löytämääsi modulia koneella.
##Tämä on infraa koodina, joten luottamusta ei tarvita. Voit lukea koodista, mitä olet ajamassa.



Lähteet:
Tero Karvinen 2022. Configuration Management Systems - Palvelinten hallinta. Luettavissa: https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/. Luettu 21.11.2022.

Joonas Kulmala 2021. Joonas Kulmala|h7|Salt My Ubuntu. Luettavissa:https://github.com/JoonasKulmala/Palvelinten-Hallinta/tree/main/h7  Luettu: 22.11.2022

Jani AUlavuo 2021. Palvelinten hallinta. Luettavissa: https://bgm064.wordpress.com/2021/04/06/palvelinten-hallinta-2021-h1/8/. Luettu: 22.11.2022

Palvelinsettiä 2021. Palvelinten hallinta harjoitus 7 oma moduli - Spring 2021. Luettavissa: https://myllys.wordpress.com/palvelinten-hallinta-harjoitus-7-oma-moduli-spring-2021 Luettu:22.11.2022
