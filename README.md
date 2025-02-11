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

```bash
sudo nano /etc/hosts
```

Ajoutez les lignes suivantes :

```bash
  GNU nano 6.2                                     /etc/hosts *                                            
127.0.0.1 localhost
127.0.1.1 vagrant

127.0.0.1      localhost.localdomain  localhost
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan      target01
192.168.56.30  target02.sandbox.lan     target02
192.168.56.40  target03.sandbox.lan       target03
```

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