# Kotitehtävä 2 Sandwich 6.4.2026 0800


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

`ansible-doc copy`: content, dest, src; owner, group, mode;
`ansible-doc apt`: name, state, update_cache.
`ansible-doc file`: path, recurse, src, state. owner, group, mode;
`ansible-doc user`: name; create_home, comment, groups, shell, state, system.
`ansible-doc authorized_key`: user, key.

https://docs.ansible.com/projects/ansible/latest/cli/ansible-doc.html#description (Viitattu 6.4.2026)

## Lähteet
https://docs.ansible.com/projects/ansible/latest/dev_guide/developing_python_3.html#unicode-sandwich-common-borders-places-to-convert-bytes-to-text-in-control-node-code

https://terokarvinen.com/passwordless-sudo/

https://xkcd.com/149/

https://terokarvinen.com/passwordless-sudo-with-ansible/
