# Kotitehtävä 2 Sandwich 6.4.2026 0800-0930 ja 2030-2155 ja 7.4.2026 1715-1815

## Johdanto
Tämä kotitehtävä oli hiukan haastava. Oli vaikea ymmärtää Ansiblen toimintaa oppintuntien perusteella. Jouduin käyttämään aikaa paljon documentaation lukemiseen ja joissain kohdissa olen kysynyt Microsoft copilottia opettamaan miten ansible toimii, jotta saan paremman ymmärryksen ohjelman toiminnasta. Luulen kuitenkin että tehtävä selkeytti hiukan miksi tunnilla tehty kansiorakenne on kuten on ja miten Playbookkia käytetään.

## Tiivistelmät

### Sudoton salasana, eikun salasanaton sudo

Sudon käyttäminen ilman salasanaa voidaan toteuttaa seuraavasti:
- Tehdään uusi käyttäjä
- Toteutetaan ryhmä johon voidaan lisätä muitakin salasanattomia sudokäyttäjiä
- Tehdään sudoers.d sisälle rivi joka käskee tietyssä ryhmässä olevien käyttäjien salasanakyselyn pois.

`sudo adduser antero`

`sudo groupadd sudoless`

`sudo adduser antero sudoless`


`cat /etc/sudoers.d/sudoless`
%sudoless ALL = (ALL) NOPASSWD: ALL

https://terokarvinen.com/passwordless-sudo/ (Viitattu 6.4.2026)

### Voileipä

- Ansible käyttää sisäisesti Unicodea (UTF‑8)

- Unicode sandwich = syöte → Unicode → käsittely Unicode-muodossa → tuloste muunnetaan tarvittaessa takaisin kohdejärjestelmän merkistöön.

https://docs.ansible.com/projects/ansible/latest/dev_guide/developing_python_3.html#unicode-sandwich-common-borders-places-to-convert-bytes-to-text-in-control-node-code (Viitattu 6.4.2026)


### Ansible-doc

Dokumentaation katseluun tehty työkalu, jonka saa esiin antamalla halutun moduulin esim:

Kommentti, jostain syystä nykyisellä Debianilla ansible-doc tarkastelu rikkoo terminaalin ja teksi häviää näkyvistä, korjausta etsitty mutta ei löydetty (0919 06.04.2026)



`ansible-doc copy`: content, dest, src; owner, group, mode;

Tiedostojen kopioiminen kohdekoneelta määritettyyn sijaintiin

- name: Copy file with owner and permissions
  
  ansible.builtin.copy:
  
    src: /srv/myfiles/foo.conf
  
    dest: /etc/foo.conf
  
    owner: foo
  
    group: foo
  
    mode: '0644'


`ansible-doc apt`: name, state, update_cache.

Hallitsee apt paketteja

- name: Install apache httpd (state=present is optional)

  ansible.builtin.apt:
  
    name: apache2
  
    state: present
  

`ansible-doc file`: path, recurse, src, state. owner, group, mode;

Tiedostojen tekeminen ja muuttaminen määritetyss sijainnissa


- name: Change file ownership, group and permissions
  
  ansible.builtin.file:
  
    path: /etc/foo.conf

    owner: foo
  
    group: foo
  
    mode: '0644'


`ansible-doc user`: name; create_home, comment, groups, shell, state, system.

Hallitsee käyttäjiä ja niiden ominaisuuksia

- name: Add the user 'johnd' with a specific uid and a primary group of 'admin'
  
  ansible.builtin.user:
  
    name: johnd
  
    comment: John Doe
  
    uid: 1040
  
    group: admin



`ansible-doc authorized_key`: user, key.

SSH avaimien lisääminen tai poistaminen käyttäjiltä

- name: Set authorized key taken from file
  
  ansible.posix.authorized_key:
  
    user: charlie
  
    state: present
  
    key: "{{ lookup('file', '/home/charlie/.ssh/id_rsa.pub') }}"


https://docs.ansible.com/projects/ansible/latest/cli/ansible-doc.html#description (Viitattu 6.4.2026)

### a) Sudoless

Loin uuden käyttäjän "santero" ja loin sudoless ryhmään johon käyttäjä lisättiin


<img width="349" height="151" alt="image" src="https://github.com/user-attachments/assets/f163758e-0e09-440e-a2f9-71151717a932" />

Avasin uuden terminaalin johon avasin roottishellin

Tämän jlkeen asetukset kondikseen sudoers.d taakse

<img width="229" height="20" alt="image" src="https://github.com/user-attachments/assets/ac5545ce-34c5-4bb2-8d39-6e8991096000" />

Tämän jälkeen SSH:lla sisään @localhost ja sudo komento joka ei kysy salasanaa, eli toimii

<img width="209" height="26" alt="image" src="https://github.com/user-attachments/assets/4b1b6415-b2b5-4fb9-8c3b-1f61a006ef52" />


(https://terokarvinen.com/passwordless-sudo/ Viitattu 6.4.2026)

### c) Salasanaton SSH

Salasanaton SSH tehty samalla tavalla kuin edellisessä tehtävässä, https://github.com/Jesaka/Palvelinten_Hallinta/blob/main/Hello_ansible.md mutta uudelle käyttäjälle 

<img width="366" height="118" alt="image" src="https://github.com/user-attachments/assets/40f93b46-978c-4ed9-a85d-47dbbdd2168f" />

### (d Asenna 2 pakettia Ansiblella

Olin tehnyt samanlaisen rakenteen Ansibleen kun edellisessä tehtävässä. Tässä kirjoitin vain uuden main.yml tiedoston (SS otettu playbookin ajamisen jälkeen kun tree on jo asentunut)

<img width="491" height="226" alt="image" src="https://github.com/user-attachments/assets/96ece0a8-52b4-45c2-a104-3c1007a127bc" />


Ja ajoin playbookin

<img width="557" height="114" alt="image" src="https://github.com/user-attachments/assets/209efbf5-dd77-49c6-b0f6-8c48b25d2100" />


`htop` Testi, paketti asentunut ja toimii


<img width="560" height="197" alt="image" src="https://github.com/user-attachments/assets/0a7e5238-827b-43e4-9502-aa06dfdc569e" />


`tree`asentunut ja toimii


<img width="302" height="113" alt="image" src="https://github.com/user-attachments/assets/563cde76-3cc2-493b-a058-d98fc921c1cf" />

### (d Filejä orjalle

Kirjoitin samaan main.yml tiedostoon toisen tehtävän. Ja kun ajoin playbookin palauttaa se tällaisen.

<img width="622" height="77" alt="image" src="https://github.com/user-attachments/assets/bc30d01f-d51d-43ec-a64d-d0261bc0a2ff" />

Ja tässä vielä lopputulos

<img width="256" height="62" alt="image" src="https://github.com/user-attachments/assets/bdc2a61d-ed38-47ae-a901-204427389660" />

### e) Jotain muuta 

Tein playbookkiin uuden käyttäjän luomisen. Ansible ei documentaation mukaan ota vastaan selkokielisiä salasanoja, joten asensin koneelle mkpasswdn

Komennot:
`sudo apt install whois`
`mkpasswd --method=sha-512 "Kirjoita tähän kohtaan salasana ilman heittomerkkejä"`

tämän jälkeen lisäsin saadun hashatun salasanan kohtaan password playbookissa


<img width="585" height="65" alt="image" src="https://github.com/user-attachments/assets/76d46324-68af-494c-adf6-dff4c5e086b6" />



## Lähteet
https://docs.ansible.com/projects/ansible/latest/dev_guide/developing_python_3.html#unicode-sandwich-common-borders-places-to-convert-bytes-to-text-in-control-node-code

https://terokarvinen.com/passwordless-sudo/

https://xkcd.com/149/

https://terokarvinen.com/passwordless-sudo-with-ansible/
