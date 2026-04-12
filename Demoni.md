#Demoni 12.4.2026 0900 -

## Tiivistelmät

### Apache asennettuna ansiblella

- Tee conf käsin ja kokeile että se toimii ennen automatisointia

  
### Handlerit

- Handlerit on tehtäviä, jotka suoritetaan vain silloin, kun jokin varsinainen tehtävä ilmoittaa niin ja aiheuttaa muutoksen.
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

Ei näy vielä, mutta saadaan oikean näköinen virheilmoitus, sivu ei näy koska konffissa olluta kansiorakennetta ja index.html tiedostoa ei ole vielä tehty.

Tein tiedoston ja uudelleenkäynnistin nginx

<img width="709" height="309" alt="image" src="https://github.com/user-attachments/assets/643fc527-4e49-4fa6-8081-bcff58872fe5" />

ja lopputuloksena 

<img width="825" height="247" alt="image" src="https://github.com/user-attachments/assets/2fd165d3-d1c7-4fc5-9ce5-2b92f3cadc3a" />

## c) Automoottorix

Seurasin tässä tehtävässä hyvin tarkasti opettajan ohjeita https://terokarvinen.com/apache-ansible/. Tehtävän tekeminen antoi hyvän käsityksen miten ansiblen rakenne toimii.

Ensimmäisenä loin kansiorakenteen ja tein tyhjät tiedostot paikalleen  (hello kansio ei ole relevantti tehtävän kannalta, vaan se on tehty aikaisemmin asniblea kokeillessa. Kansiot jota tehtävässä on tarvittu on kaikki muut paitsi nginx.

<img width="626" height="297" alt="image" src="https://github.com/user-attachments/assets/48009511-9b9b-4c39-91c2-7e8d3c7dc8f4" />

Seuraavaksi tein tiedostot ja muutin tarvittavat polut omaan järjestelmään sopivaksi (SS on otettu kun tehtävä on tehty joten become:yes joka mainitaan seuraavassa kohdassa on jo paikallaan.

<img width="914" height="559" alt="image" src="https://github.com/user-attachments/assets/b2344f71-795c-4251-8ca0-525ba412bb7c" />


Tässä kohti ajoin playbookin ja sain seuraavan virheilmoituksen. Ilmeisesti ansible tarvitsee erillisen käskyn käyttää sudoa, kun se asentaa `apt` komennolla paketteja. Korjattu laittamalla tasks alle become:yes rivi.

<img width="1143" height="254" alt="image" src="https://github.com/user-attachments/assets/9e2e1928-95a9-418f-b86f-972c13743f2f" />

Ansible kysyy jostain syystä sudon salasanaa vaikka tämä pitäisi olla jo tehtynä siten että käyttäjältä ei kysytä sudo salasanaa korjattu lisäämällä käyttäjä sudoless ryhmään joka on tehty raportissa https://github.com/Jesaka/Palvelinten_Hallinta/blob/main/Hello_ansible.md

<img width="1107" height="159" alt="image" src="https://github.com/user-attachments/assets/65ae3f7e-e537-4b60-bcac-b227dab86b2d" />

Lopputuloksena ajettu playbook toimii oikein, changed näkyy = 0, koska se on tehty localhostiin ja samalle käyttäjälle -> Kun mikään ei muutu niin tiedostot ovat omalla samalla paikallaan ja homma toimii yhä

<img width="1281" height="361" alt="image" src="https://github.com/user-attachments/assets/75484f55-c995-45ce-9963-d127b997ef21" />



## Lähteet

https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html
https://terokarvinen.com/apache-ansible/
https://nginx.org/en/docs/beginners_guide.html
https://github.com/Jesaka/Palvelinten_Hallinta/blob/main/Hello_ansible.md




