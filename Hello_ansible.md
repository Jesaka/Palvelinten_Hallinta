# Hello ansible 08:12 30.3.2026 - 10:30 30.3.2026

## Tiivistelmä SSH Public key


### (A ja B SSH:n asennus ja käyttöönottto

#### 1. Asennetaan OpenSSH

`sudo apt-get update`

`sudo apt-get -y install ssh`


Käynnistetään ja otetaan palvelu käyttöön automaattisesti 

`sudo systemctl enable --now ssh`


#### 2. SSH-yhteyden testaus

Testataan kirajutuminen esimerkiksi local hostiin

`ssh localhost`


#### 3. Public key luominen

Generoidaan avain

`ssh-keygen`

kopioidaan avain käyttäjälle

`ssh-copy-id localhost`


Syötetään salasana vielä kerran ja tämän jälkeen 


`ssh localhost` pitäisi kirjautua sisään ilman salasanaa

(https://terokarvinen.com/ssh-public-key-login-without-password/)

## (C H1 Hello Ansible


### Ansible asennus

Komento kuten opintomateriaaleissa (https://terokarvinen.com/hello-ansible/)

`sudo apt-get install ansible micro bash-completion tree`

Ansible on asennettu. 

### SSH testaaminen Ansiblen kanssa

Tehdään Ansiblen konffille oma kansio

<img width="147" height="41" alt="image" src="https://github.com/user-attachments/assets/e2c5ea49-e645-44ef-9273-5ca9d944a194" />

Seuraavaksi luodaan lista hosteista ja lisätään sinne localhost

<img width="156" height="41" alt="image" src="https://github.com/user-attachments/assets/c554d658-782f-4927-a535-1bcf93edb6c4" />

Ja kokeillaan ajaa komento Ansiblella kaikille hosteille


<img width="564" height="71" alt="image" src="https://github.com/user-attachments/assets/20163f72-ed67-4ba1-bfb5-271849d7ea5a" />


Ansible antoi varoituksen Pythonin tulkista, mutta tässä vain ilmoitetaan että tällainen löytyi ja sitä käytetään. Jos tulevaisuudessa on asennettu jotain muuta niin saatetaan käyttää sitä. Ja uptime tulin näkyviin joten kaikki ok.


### Hosts.ini käyttäminen asetaaminen konffiin

Luomalla ansible.cfg tiedosto ja asetamalla invetory=hosts.ini, voimme ajaa komennon `ansbile all -a "uptime"` mainitsematta hosts.ini tiedostoa. 

<img width="566" height="131" alt="image" src="https://github.com/user-attachments/assets/2a900ed8-1c08-44f2-9dba-8e11824ce12a" />


Kommentti: Tajusin tässä vaiheessa että python varoituksen poistaminen oli unohtunut joten se on tehty tässä vaiheessa.

<img width="350" height="106" alt="image" src="https://github.com/user-attachments/assets/e8f900c4-e48f-4fd6-a561-1672ce8d0256" />


### Site.yml

Tässä vaiheessa huomaan etten enään pysy kovin hyvin kärryillä mitä tapahtuu. En päässyt ensimmäiselle tunnille zoom ongelmien vuoksi, joten tästä eteenpäin noudatan puhtaasti vain ohjeita sen syvemmin ymmärtämättä mitä teen.

<img width="584" height="101" alt="image" src="https://github.com/user-attachments/assets/7aef0cdf-4382-475e-9ed5-42c3a2460fd3" />

### Tiedoston luonti 


Tehtävä näyttää onnistuneen samalla tavalla kuin opettajan ohjeissa.


<img width="589" height="209" alt="image" src="https://github.com/user-attachments/assets/4c0be060-2e1a-4f59-93fc-40f93a0fddad" />

### Näytä mitä teet

Hei! näillä saatiin komento näyttämään mitä verhon takana tapahtuu.

<img width="586" height="238" alt="image" src="https://github.com/user-attachments/assets/f93f68d1-ff87-400b-8b3d-baf3de05fd81" />

## MUISTIINPANOJA TUNNILTA MUOKATTU 1.4

<img width="664" height="228" alt="image" src="https://github.com/user-attachments/assets/1bc3d3f5-7f45-4922-a3c2-3371b1a8c3d4" />



### Lähteet 

https://terokarvinen.com/ssh-public-key-login-without-password/

https://terokarvinen.com/hello-ansible/
