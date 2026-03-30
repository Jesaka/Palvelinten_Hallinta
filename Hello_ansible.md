# Hello ansible

## Tiivistelmä SSH Public key


### SSH:n asennus ja käyttöönottto

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

## H1 Hello Ansible


### Installing Ansible

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


Ansible antoi varoituksen Pythonin tulkista, mutta tässä vain ilmoitetaan että tällainen löytyi ja sitä käytetään. Jos tulevaisuudessa on asennettu jotain muuta niin saatetaan käyttää sitä. Ja uptime tulin näkyviin joten kaikki ok. En koe varoituksen poistamista tarpeelliseksi kunhan ymmärtää mistä se johtuu.


### Hosts.ini käyttäminen asetaaminen konffiin

Luomalla ansible.cfg tiedosto ja asetamalla invetory=hosts.ini, voimme ajaa komennon `ansbile all -a "uptime"` mainitsematta hosts.ini tiedostoa. 

<img width="566" height="131" alt="image" src="https://github.com/user-attachments/assets/2a900ed8-1c08-44f2-9dba-8e11824ce12a" />









### Lähteet 
https://terokarvinen.com/ssh-public-key-login-without-password/
https://terokarvinen.com/hello-ansible/
