# Kotitehtävä 2 Sandwich 6.4.2026 0800-0930 ja 


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

## Lähteet
https://docs.ansible.com/projects/ansible/latest/dev_guide/developing_python_3.html#unicode-sandwich-common-borders-places-to-convert-bytes-to-text-in-control-node-code

https://terokarvinen.com/passwordless-sudo/

https://xkcd.com/149/

https://terokarvinen.com/passwordless-sudo-with-ansible/
