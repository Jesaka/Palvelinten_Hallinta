# Hello ansible

## Tiivistelmä SSH Public key


### SSH:n asennus ja käyttöönottto

#### 1. Asennetaan OpenSSH <br></br>

**sudo apt-get update**

**sudo apt-get -y install ssh**


Käynnistetään ja otetaan palvelu käyttöön automaattisesti 

**sudo systemctl enable --now ssh**

<br></br>

#### 2. SSH-yhteyden testaus

Testataan kirajutuminen esimerkiksi local hostiin

**ssh localhost**

<br></br>

#### 3. Public key luominen

Generoidaan avain

**ssh-keygen**

kopioidaan avain käyttäjälle

**ssh-copy-id localhost**

Syötetään salasana vielä kerran ja tämän jälkeen 

**ssh localhost** pitäisi kirjautua sisään ilman salasanaa






### Lähteet 
https://terokarvinen.com/ssh-public-key-login-without-password/
