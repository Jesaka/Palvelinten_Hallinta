#Demoni 12.4.2026 0900 -

## Tiivistelmät

### Apache asennettuna ansiblella

- Tee conf käsin ja kokeile että se toimii ennen automatisointia
- Selkeä puurakenne helpottaa tekemistä:

  
roles/apache2/  # All files related to "apache2" role
├── files/      # Master copies of files
│   └── example.com.conf
├── handlers/   # restarting services
│   └── main.yml
└── tasks/      # most ansible code
    └── main.yml
### Handlerit

- Handlerit ovat erityisiä tehtäviä, jotka suoritetaan vain silloin, kun jokin varsinainen tehtävä ilmoittaa niille (notify) ja aiheuttaa muutoksen.
- Niitä käytetään tyypillisesti palveluiden uudelleenkäynnistämiseen, konfiguraation lataamiseen tai muihin toimenpiteisiin, joita ei haluta tehdä turhaan jokaisella ajolla.
- Useat tehtävät voivat ilmoittaa samalle handlerille, mutta handler suoritetaan silti vain kerran kunkin play‑osion lopussa.

### Ansible.builtin.service

- Moduuli jolla hallitaan palveluiden tilaa, tekee käytännössä samaa kuin `systemctl`

## Tehtävät

### a) Demoni

Apache2 oli jo asennettuna tunnin jäljiltä, joten tein vain uudet konfiguraatiotiedostot

<img width="727" height="206" alt="image" src="https://github.com/user-attachments/assets/32860470-7ec0-4ffa-8396-f38eed06d2b3" />

Tässä välissä kun conffit on tehty sudo a2 `sudo a2dissite 000-default.conf` Deafult sivu pois päältä `sudo a2ensite sivun.konftiedoston.nimi.conf` Uusi sivu päälle  ja  `sudo systemctl restart apache2` niin uudet asetukset tallentuvat 

<img width="645" height="59" alt="image" src="https://github.com/user-attachments/assets/946d3d0c-9b0c-4feb-9490-f66f26f823d5" />

Ja sivu näkyy localhostina

<img width="1277" height="336" alt="image" src="https://github.com/user-attachments/assets/ef4f3d38-86c0-44f3-8fff-5384c100260a" />



### b) Moottorix 

Aloitin asentamalla nginx konmennolla `sudo apt install nginx`

kun nginx oli asentunut menin /etc/nginx/sites-available ja korvasin default sivun omallani. 

<img width="669" height="222" alt="image" src="https://github.com/user-attachments/assets/ff2ab796-3495-4f61-a7b3-80a5e153ba78" />

Kun konffi oli tehty, tein symlinkin tiedostoon `sudo ln -s /etc/nginx/sites-available/pallo.com /etc/nginx/sites-enabled/pallo.com`

Tässä kohti sain muutaman virheilmoituksen syntaksista (Tarkistaminen `sudo nginx -t`) ja pyysin copilottia kertomaan virheet jotta voin korjata ne.

Kun konffi oli tehty `sudo sytemctl restart nginx` ja tarkistin näkyykö sivu.

<img width="1275" height="236" alt="image" src="https://github.com/user-attachments/assets/e62e5b63-bc2a-49ea-8a4e-f32f8464e8f4" />

Ei näy vielä, mutta saadaan oikean näköinen virheilmoitus, sivu ei näy koska konffissa olluta kansiorakennetta ja index.html tiedostoa ei ole vielä tehty











# NäÄMÄ ON MUISTIINPANOJA
Apache2 .conf tiedosto
<img width="284" height="103" alt="image" src="https://github.com/user-attachments/assets/34c0084d-286c-42b2-9614-0e5b0e060a36" />


<img width="510" height="327" alt="image" src="https://github.com/user-attachments/assets/8255bfab-8dc7-47dc-b5fe-2db0fa687ca0" />

Master slave arkkitehtuuri COPY:n DEST määrittää mihin orja saa uuden tiedoston. Kansiorakenteessa on taas määritetty että tiedoston src on files kansiossa KOPIOI TÄMÄ TIEDOSTO



<img width="897" height="277" alt="image" src="https://github.com/user-attachments/assets/049a7559-ef8d-446d-8e49-570b7ca16c81" />


<img width="1135" height="641" alt="image" src="https://github.com/user-attachments/assets/727867cd-a474-4136-89b4-8ad74357862b" />


