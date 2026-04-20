# Pizza fantasia 20.4,2026 1915

## Tiivistelmä
- DSL voi kasvaa helposti isoksi ja vaikeasti ymmmärrettäväksi.
- Todellisuudessa vain kourallinen komentoja kattaa valtaosan käytöstä
-  Konfigurointikielen tulisi pyrkiä olemaan yleiskieltä tai lähellä sitä.

  https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks (Kappaleet 4.12.1-3)


## Räpylä


### Lähtötilanne
Aloitin pysäyttämällä nginx ja apache2. Tämän jälkeen poistin vielä molempien sites-enabled kansiosta aiemmin tekemäni sivut.

Kun sivut oli poistettu, tarkistin localhostin `shift+refresh`


<img width="1272" height="570" alt="image" src="https://github.com/user-attachments/assets/e6cc3f80-5ec5-415a-a637-977210f6d558" />


### Caddyn asentaminen

Hain Caddyn dokumentaation auki ja avasin ensimmäisenä Install osion. Asentaminen tapahtui antamalla seuraavat komennot

`sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl`

`curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg`

`curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list`

`sudo chmod o+r /usr/share/keyrings/caddy-stable-archive-keyring.gpg`

`sudo chmod o+r /etc/apt/sources.list.d/caddy-stable.list`

`sudo apt update`

`sudo apt install caddy`

Tämän jälkeen annoin komennon `caddy run` (Ilmeisesti Caddy oli jo käynnistynyt kun virheilmoitus on address allready in use). Tämän jälkeen Caddyn ohjeiden mukaan komento `curl localhost:2019/config/` joka näköjään palautti JSONIN. Kaiken järjen mukaan siis homma rokkaa ja voidaan jatkaa.

<img width="1682" height="125" alt="image" src="https://github.com/user-attachments/assets/d8424f1d-8294-416a-b852-cc1aebc5d0d9" />


### Konfigurointi

Olen aikaisemmin tehnyt vastaavaa Apachella ja Nginx:llä, joten olin kiinnostunut mitä /etc/caddy on syönyt. Sisältä löytyi 1 tiedosto "Caddyfile". Tiedoston avatessa se näytti hyvin samalta kun apachen tai nginx config file.

<img width="703" height="488" alt="image" src="https://github.com/user-attachments/assets/220e978b-3289-4fdc-be9a-7a949d260236" />


Muutin tiedostoon oman kansiorakenteet /usr/share/caddy tilalle ja loin sinne index.html tiedoston. (Chat GPT:lle annettu prompt: "Luo yksinkertainen html sivu, jota Caddy voi käyttää index.html tiedsotona")

Tämän jälkeen uudelleenkäynnistin demonin komennolla `sudo systemctl restart caddy`


<img width="1280" height="336" alt="image" src="https://github.com/user-attachments/assets/6529ca64-29ed-4c43-9877-13d019d77eb8" />





## Muistilistaa Automatisointiin.

1. Asennus (Pitääkö komennot ajaa uudestaan?)
2. Config file on /etc/caddy/Caddyfile ja sen sisältö:
   
# The Caddyfile is an easy way to configure your Caddy web server.
#
# Unless the file starts with a global options block, the first
# uncommented line is always the address of your site.
#
# To use your own domain name (with automatic HTTPS), first make
# sure your domain's A/AAAA DNS records are properly pointed to
# this machine's public IP, then replace ":80" below with your
# domain name.

:80 {
	# Set this path to your site's directory.
	root * /usr/share/caddy

	# Enable the static file server.
	file_server

	# Another common task is to set up a reverse proxy:
	# reverse_proxy localhost:8080

	# Or serve a PHP site through php-fpm:
	# php_fastcgi localhost:9000
}

# Refer to the Caddy docs for more information:
# https://caddyserver.com/docs/caddyfile

3. Index.html paikalleen.

## Lähteet 

https://caddyserver.com/docs/install#debian-ubuntu-raspbian

https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks

https://terokarvinen.com/palvelinten-hallinta/
