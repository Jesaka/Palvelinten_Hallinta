# Pizza fantasia 20.4,2026 1915 2130. Jatkettu 21.4.2026 0730- 1000

## Johdanto

Tämä harjoitustyö haastoi todella. Automatisointi ja verkkosivun esiin saaminen onnistuu jo hyvin. Uuteen ohjelmaan vaihtamisessa huomaa että siirrettävää osaamista on kertynyt Ansiblen ja Nginx käytöstä. Suurimpana haastena oli Caddyn saaminen uudelleen käynnistääm. Jostain syystä Caddy taistelee handlerin uudelleen käynnistystä vastaan ja se jouduttiin lopulta toteuttamaan raakana shellkomentona ansible.builtin.shell käyttäen.

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


Muutin tiedostoon oman kansiorakenteet /usr/share/caddy tilalle ja loin /home/santero/caddy sisälle index.html tiedoston. (Chat GPT:lle annettu prompt: "Luo yksinkertainen html sivu, jota Caddy voi käyttää index.html tiedsotona")

Tämän jälkeen uudelleenkäynnistin demonin komennolla `sudo systemctl restart caddy`


<img width="1280" height="336" alt="image" src="https://github.com/user-attachments/assets/6529ca64-29ed-4c43-9877-13d019d77eb8" />

### Automatisointi

Jos haluat lukea enemmän vaihe vaiheelta miten automatisointi on tehty niin olen toiminut samalla tavalla kuin edellisessä raportissa:

https://github.com/Jesaka/Palvelinten_Hallinta/blob/main/Demoni.md

Tässä raportissa käsitellään lähinnä syntyneitä ongelmia ja Caddyn käyttämiseen liittyviä uusia opittuja asioita.

Aloitin tekemällä Saman kansiorakenteen kun aikaisemmin toimivaksi todetusti nginx:ää automatisoidessa.

<img width="627" height="245" alt="image" src="https://github.com/user-attachments/assets/0745d9c2-36b0-45d1-a030-799c0e800848" />

Tämän jälkeen loin tarvittavat tiedostot. Kun koko paketti oli luotu lähdin korjaamaan virheitä. Ansible antoi joka playbook ajolla muutamia virheitä, en käsittele kaikkia toistuvia erikseen koska korjasin lähinnä 2 asiaa:

1. Kirjoitusvirheet main.yml tiedostoissa
2. Become: yes lisääminen taskeihin kun pitää sudottaa prosessi.


### Ongelma: Caddy conffi ei mene sisään ansible ajossa.

Vikakuvaus: Ajaessa Ansiblen playbookki saan takaisin virheilmoituksen Caddy reload time outtaa. Koitin tehdä handlerssiin myös restart komennon, joka tuottaa saman lopputuloksen.


<img width="1163" height="357" alt="image" src="https://github.com/user-attachments/assets/61d395ef-5c2f-4bf9-b1b5-f5a54a6f444a" />


Toimpenpiteet: 
- Tarkistin että kaikki polut annetaan tiedostoissa oikein. (Ei ratkaisua)
- Tarkistin että ansiblen ajo on todellisuudessa uudelleenkirjoittanut tiedoston (Ei ratkaisu)
- Tarkistin että directoryilla on  x oikeudet ja index.html tiedosto on others luettavissa. (Ei ratkaisu)
- Pallottelin tekoälyn kanssa asiaa ja niinkuin arvata saattaa sain paljon outoja vikakorjauksia (Tunti elämästäni jota en saa takaisin), mutta lopulta copilot osasi sanoa että luomani /home/santero/caddy kansio tarvitsee mtyös read oikeuden. Tämä ratkaisi ongelman. En muista puhuttiinko tunnilla että tällaiselle olisi tarve, jos käytiin olin täysin unohtanut sen. Mutta viimein sain sivun päivittymään siihen minkä ansible oli ajanut sisään.


<img width="1277" height="349" alt="image" src="https://github.com/user-attachments/assets/6b104d0e-d118-4020-98d9-49a7c6e5373e" />

## Asetus

1. Kävin poistamassa index.html tiedoston

   <img width="533" height="144" alt="image" src="https://github.com/user-attachments/assets/9b9954d4-326b-4fe4-8622-2860ee811c6d" />

3. kävin poistamassa caddyfilen

<img width="708" height="69" alt="image" src="https://github.com/user-attachments/assets/0bc4913a-8a6e-46ae-a041-c38d7d611f48" />

Kun ajoin playbookin uudelleen sain saman varoituksen kun aikaisemmin

<img width="1785" height="603" alt="image" src="https://github.com/user-attachments/assets/fee80a57-e334-4400-ba86-f34b7d031709" />

Mutta tällä kertaa sivu päivittyy niinkuin pitääkin

<img width="1276" height="480" alt="image" src="https://github.com/user-attachments/assets/c1028ee5-72b5-4766-a31e-f064f9da5c59" />

Tässä kohtaa joudun kellistämään vatsan ja kysymään joltain viissaammalta en löydä mitään korjausta en googlamalla, en kahlaamalla dokumentaatiota enkä kysymällä tekohölmölöltä. Kun arvioit raporttia ja näet tämän niin kommentoi ihmeessä korjausehdotuksia jos niitä on.

## Remontti

Ajoin `sudo apt-get purge caddy`

<img width="1277" height="711" alt="image" src="https://github.com/user-attachments/assets/6ae3bc42-00ea-476e-b943-e451591bd0c1" />

<img width="1782" height="610" alt="image" src="https://github.com/user-attachments/assets/1b66d99e-7187-4867-b310-c3e113045ae5" />



ja ajoin playbookin uudelleen


Localhost näyttää nyt default sivua -> todennäköisesti koska en saa Demonia uudelleenkäynnistymään Ansiblella
<img width="1228" height="822" alt="image" src="https://github.com/user-attachments/assets/b71526fb-de8d-4bd1-b8db-cb46b2a3af55" />

Annoin komennon `sudo systemctl daemon-reload` ja `sudo systemctl restart caddy`

Tämän jälkeen sivu muuttui takaisin siksi joka on Ansiblelle annettu.

Oma johtopäätös: Ansible laittaa tiedostot nyt oikeille paikoille ja antaa niille oikeat oikeudet. Ratkaistavaksi jää miten saan demonin käynnistämään itsensä handlerilla.

<img width="1263" height="313" alt="image" src="https://github.com/user-attachments/assets/8f9cfb7f-459e-4fc3-9b67-d5dd6324a2d7" />

Ratkaisu:

Tein ansible palybookkiin raakana shell komentona komennot

````
- name: Reload systemd daemon
  ansible.builtin.shell: sudo systemctl daemon-reload
  
    
- name: Restart Caddy via shell
  ansible.builtin.shell: sudo systemctl restart caddy
````

Tämän jälkeen purgesin caddyn ja poistin index.html uudelleen ja lopputuloksena 

<img width="1253" height="514" alt="image" src="https://github.com/user-attachments/assets/87e5093c-3424-4147-b8d5-92914590f6d9" />

<img width="1261" height="389" alt="image" src="https://github.com/user-attachments/assets/19ef0af1-c7da-47d1-8df4-6ad59a38b86c" />


Tila osoittuu myös täsää melkein impodentiksi koska ainoat muutokset on shellillä pakotetut raakakomennot. Tähän kotiläksyyn en saanut ratkaistua miten saan Ansiblella Caddyn uudelleenkäynnistymään vain tarvittaessa, mutta uusia oppimiskokemuksia tuli raakakomentojen käytöstä


<img width="1760" height="472" alt="image" src="https://github.com/user-attachments/assets/e79eee89-7043-4f12-8568-81048f1d7193" />







## Muistilistaa Automatisointiin.

1. Asennus (Jos homma tehtäisiin alusta asti niin on mahdollista että caddyn asentamisessa käytetyt komennot tulisi ajattaa ansiblen kautta. Itsellä heräsi kysymys miten tämä vaikuttaisi impedanssiin?)
2. Config file on /etc/caddy/Caddyfile ja sen sisältö:
   
``` Caddyfile
:80 {

	root * /usr/share/caddy

	# Enable the static file server.
	file_server

	# Another common task is to set up a reverse proxy:
	# reverse_proxy localhost:8080

	# Or serve a PHP site through php-fpm:
	# php_fastcgi localhost:9000
}
```
3. Index.html paikalleen.
4. Tarkista kansiorakenteen oikeudet, minulla index.html hakemisto tarvitsi r oikeuden.

## Lähteet 
https://github.com/Jesaka/Palvelinten_Hallinta/blob/main/Demoni.md

https://caddyserver.com/docs/install#debian-ubuntu-raspbian

https://caddyserver.com/docs/quick-starts

https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks

https://terokarvinen.com/palvelinten-hallinta/
