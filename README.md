# formation-ansible-ema

## Atelier 1
### Exercice 1

Démarrez la VM ubuntu depuis le répertoire atelier-01.

```bash
[ema@localhost:atelier-01] $ pwd
/home/ema/formation-ansible-ema/formation-ansible/atelier-01

[ema@localhost:atelier-01] $ vagrant up ubuntu
```

Connectez-vous à cette VM.

```bash
[ema@localhost:atelier-01] $ vagrant ssh ubuntu
```

Rafraîchissez les informations sur les paquets.

```bash
[vagrant@ubuntu] $ sudo apt update
```

Recherchez le paquet ansible avec les options qui vont bien.

```bash
[vagrant@ubuntu] $ sudo apt search ansible
```

Installez le paquet officiel fourni par la distribution.

```bash
[vagrant@ubuntu] $ sudo apt install ansible
```

Vérifiez si l’installation s’est bien déroulée.

```bash
[vagrant@ubuntu] $ ansible --version
```

Notez la version d’Ansible.

```bash
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0]
```

Déconnectez-vous et supprimez la VM.

```bash
[vagrant@ubuntu] $ exit

[ema@localhost:atelier-01] $ vagrant destroy -f
```

### Exercice 2

Répétez l’exercice précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible :

$ sudo apt-add-repository ppa:ansible/ansible

Notez la version fournie par ce dépôt tiers et comparez avec la version officielle de l’exercice précédent.


```bash
[ema@localhost:atelier-01] $ pwd
/home/ema/formation-ansible-ema/formation-ansible/atelier-01

[ema@localhost:atelier-01] $ vagrant up ubuntu
```

Connectez-vous à cette VM.

```bash
[ema@localhost:atelier-01] $ vagrant ssh ubuntu
```

Rafraîchissez les informations sur les paquets.

```bash
[vagrant@ubuntu] $ sudo apt update
```

Ajoutez le dépôt PPA pour Ansible.

```bash
[vagrant@ubuntu] $ sudo apt-add-repository ppa:ansible/ansible
```

Rafraîchissez les informations sur les paquets.

```bash
[vagrant@ubuntu] $ sudo apt update
```

Installez le paquet officiel fourni par la distribution.

```bash
[vagrant@ubuntu] $ sudo apt install ansible
```

Vérifiez si l’installation s’est bien déroulée.

```bash
[vagrant@ubuntu] $ ansible --version
```

Notez la version d’Ansible.

```bash
ansible [core 2.17.8]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

Déconnectez-vous et supprimez la VM.

```bash
[vagrant@ubuntu] $ exit

[ema@localhost:atelier-01] $ vagrant destroy -f
```

### Exercice 3

Lancez une VM Rocky Linux et installez Ansible en utilisant PIP et Virtualenv.

Important: Notez bien que contrairement à Debian, le paquet python3-venv n’est pas nécessaire ici, étant donné que Virtualenv fait partie des modules standard de Python dans cette distribution.

```bash
[ema@localhost:atelier-01] $ pwd
/home/ema/formation-ansible-ema/formation-ansible/atelier-01

[ema@localhost:atelier-01] $ vagrant up rocky
```

Connectez-vous à cette VM.

```bash
[ema@localhost:atelier-01] $ vagrant ssh rocky
```

Rafraîchissez les informations sur les paquets.

```bash
[vagrant@rocky] $ sudo dnf check-update
```

Installez les paquets nécessaires pour installer Ansible avec PIP et Virtualenv.

```bash
[vagrant@rocky] $ sudo dnf install python3-pip
```

Installez Ansible avec PIP et Virtualenv.

```bash
[vagrant@rocky] $ python3 -m venv ~/.venv/ansible
[vagrant@rocky] $ source ~/.venv/ansible/bin/activate
(ansible) [vagrant@rocky] $ pip install ansible
```

Vérifiez si l’installation s’est bien déroulée.

```bash
(ansible) [vagrant@rocky] $ ansible --version
```

Notez la version d’Ansible.

```bash
(ansible) [vagrant@rocky ~]$ ansible --version
ansible [core 2.15.13]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/vagrant/.venv/ansible/lib64/python3.9/site-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/vagrant/.venv/ansible/bin/ansible
  python version = 3.9.18 (main, Sep  7 2023, 00:00:00) [GCC 11.4.1 20230605 (Red Hat 11.4.1-2)] (/home/vagrant/.venv/ansible/bin/python3)
  jinja version = 3.1.5
  libyaml = True
```

### Atelier 3 - Exercice

Faites le nécessaire pour réussir un ping Ansible comme ceci :

```bash
$ ansible all -i target01,target02,target03 -m ping
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
Modifiez le fichier /etc/hosts pour ajouter les noms de domaine des machines cibles.

```bash
sudo nano /etc/hosts
```

Ajoutez les lignes suivantes :

```bash
127.0.0.1 localhost
127.0.1.1 vagrant

127.0.0.1      localhost.localdomain  localhost
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan      target01
192.168.56.30  target02.sandbox.lan     target02
192.168.56.40  target03.sandbox.lan       target03
```

Ajoutez les clés SSH des machines cibles.

```bash
ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
```

```bash
ssh-keygen
```

```bash
vagrant@control:~$ ssh-copy-id vagrant@target01
vagrant@control:~$ ssh-copy-id vagrant@target02
vagrant@control:~$ ssh-copy-id vagrant@target03
```

```bash
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```


### Atelier 6 - Exercice

```bash
vagrant up

vagrant ssh control
```



- Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.
Ajoutez les lignes suivantes à /etc/hosts :

```bash
192.168.56.20 target01
192.168.56.30 target02
192.168.56.40 target03
```

- Configurez l’authentification par clé SSH avec les trois Target Hosts.
```bash
vagrant@control:~$ ssh-keygen
vagrant@control:~$ ssh-copy-id vagrant@target01
vagrant@control:~$ ssh-copy-id vagrant@target02
vagrant@control:~$ ssh-copy-id vagrant@target03
```

- Installez Ansible.
```bash
vagrant@control:~$ sudo apt update
vagrant@control:~$ sudo apt install ansible
```

- Envoyez un premier ping Ansible sans configuration.
```bash
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
```

- Créez un répertoire de projet ~/monprojet.
```bash
vagrant@control:~$ mkdir ~/monprojet
```

- Créez un fichier vide ansible.cfg dans ce répertoire.

```bash
vagrant@control:~$ touch ~/monprojet/ansible.cfg

[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```

- Vérifiez si ce fichier est bien pris en compte par Ansible.

Il faut bien être dans le répertoire de projet pour que le fichier ansible.cfg soit pris en compte.

```bash
vagrant@control:~$ ansible --version | head -n 2
ansible 2.10.8
  config file = None
vagrant@control:~$ cd monprojet/
vagrant@control:~/monprojet$ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```

- Spécifiez un inventaire nommé hosts.

```bash
vagrant@control:~/monprojet$ touch hosts

monprojet/hosts
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant

```


- Activez la journalisation dans ~/journal/ansible.log.
```bash
[defaults]
log_path = ~/journal/ansible.log
```

- Testez la journalisation.

```bash
vagrant@control:~/monprojet$ ansible all -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

```bash
vagrant@control:~/monprojet$ cat ~/journal/ansible.log
2025-02-12 09:32:45,050 p=3847 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-02-12 09:32:45,065 p=3847 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-02-12 09:32:45,086 p=3847 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

- Créez un groupe [testlab] avec vos trois Target Hosts.
- Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
  
```bash
hosts

[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
```

- Envoyez un ping Ansible vers le groupe de machines [all].
```bash
ansible all -m ping
```

- Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.

```bash
hosts

[testlab:vars]
ansible_user=vagrant
ansible_become=yes
```

- Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.

```bash
vagrant@control:~$ ansible all -a "head -n 1 /etc/shadow"
```

```bash
vagrant@control:~/monprojet$ ansible all -a "head -n 1 /etc/shadow"
target02 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target01 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
```

- Quittez le Control Host et supprimez toutes les VM de l’atelier.

```bash
vagrant@control:~$ exit
[ema@localhost:atelier-06] $ vagrant destroy -f
```
