
# Challenge 1 : Préparer l'environnement

## Étapes à suivre :

1. **Démarrez les VM**
   ```bash
    vagrant up
    vagrant ssh control
    ```

2. **Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d'hôte simple.**
   ```bash
    192.168.56.10 control 
    192.168.56.20 target01
    192.168.56.30 target02
    192.168.56.40 target03
   ```

3. **Configurez l'authentification par clé SSH avec les trois Target Hosts.**
    ```bash
    ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
    ```
    ```
    # target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
    # target01:22 SSH-2.0-OpenSSH_8.7
    # target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
    ```
    ```bash
    ssh-keygen
    ```
    ```
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/vagrant/.ssh/id_rsa
    Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:aYovMc7xl2jwF+kod+3k0KT0LXzyiBbOUmfUuNb1BaM vagrant@control
    The key's randomart image is:
    +---[RSA 3072]----+
    |                 |
    |              o  |
    |           o . o |
    |         .o E . .|
    |        So.o . ..|
    |    =. +=*=..   .|
    |   o.B.BoOB o    |
    |    =.O X=.*     |
    |     =.*..+ .    |
    +----[SHA256]-----+
    ```
    ```bash
    vagrant@control:~$ ssh-copy-id vagrant@target01

    Number of key(s) added: 1

    vagrant@control:~$ ssh-copy-id vagrant@target02

    Number of key(s) added: 1

    vagrant@control:~$ ssh-copy-id vagrant@target03

    Number of key(s) added: 1
    ```
4. **Installer Ansible**
    ```bash
    sudo apt update
    apt-cache search --names-only ansible
    sudo apt install -y ansible
    ansible --version
    ```
    ```
    ansible 2.10.8
    config file = None
    configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python3/dist-packages/ansible
    executable location = /usr/bin/ansible
    python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0]
    ```
4. **Envoyez un premier ping Ansible sans configuration**
    ```bash
    ansible all -i target01,target02,target03 -m ping
    ```
    ```
    target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
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
    target02 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    ```
5. **Créez un répertoire de projet ~/monprojet**
    ```bash
    mkdir ~/monprojet
    ```
6. **Créez un fichier vide ansible.cfg dans ce répertoire.**
    ```bash
    touch ~/monprojet/ansible.cfg
    ```
7. **Vérifiez si ce fichier est bien pris en compte par Ansible.**
    ```bash
    ansible --version | head -n 2
    ``` 
    ```
    ansible 2.10.8
        config file = /home/vagrant/monprojet/ansible.cfg
    ```
8. **Spécifiez un inventaire nommé hosts.**
    ```bash
    nano hosts
    ``` 
    ```
    [testlab]
    target01
    target02
    target03
    ```
9. **Activez la journalisation dans ~/journal/ansible.log.**
    ```bash
    mkdir -v ~/logs
    nano ansible.cfg
    ``` 
    ```
    [defaults]
    inventory = ./hosts    
    log_path = ~/logs/ansible.log
    ```
10. **Testez la journalisation.**
    ```bash
    ansible all -i target01,target02,target03 -m ping
    cat ~/logs/ansible.log
    ``` 
    ```
    2025-11-06 08:56:50,657 p=3476 u=vagrant n=ansible | [WARNING]: Platform linux on host target01 is using the discovered Python interpreter at /usr/bin/python, but
    future installation of another Python interpreter could change the meaning of that path. See
    https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information.

    2025-11-06 08:56:50,658 p=3476 u=vagrant n=ansible | target01 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python"
        },
        "changed": false,
        "ping": "pong"
    }
    2025-11-06 08:56:50,787 p=3476 u=vagrant n=ansible | target03 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    2025-11-06 08:56:50,796 p=3476 u=vagrant n=ansible | target02 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    ```
11. **Créez un groupe [testlab] avec vos trois Target Hosts**
    ```bash
    nano hosts
    ``` 
    ```
    [testlab]
    target01
    target02
    target03
    ```
12. **Définissez explicitement l'utilisateur vagrant pour la connexion à vos cibles.**
    ```bash
    nano hosts
    ``` 
    ```
    [testlab:vars]
    ansible_python_interpreter=/usr/bin/python3
    ansible_user=vagrant

    ```
13. **Envoyez un ping Ansible vers le groupe de machines [all].**
    ```bash
    ansible all -m ping
    ``` 
    ```
    target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
    }
    target03 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    target02 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    ```
14. **Définissez l'élévation des droits pour l'utilisateur vagrant sur les Target Hosts.**
    ```bash
    nano hosts
    ```
    ```
    ansible_become=yes
    ```
15. **Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.**
    ```bash
    ansible all -a "head -n 1 /etc/shadow"
    ```
    ```
    target01 | CHANGED | rc=0 >>
    root:$6$RhLTeLPrOa8atE7M$XlElJCnmheVLp2p9Gybrs7Zr2.X4dKjV5h5WUzPCe.dO016Hf18hUFaal5rw3WFFhtWnLFk55HV01tamofh1G.::0:99999:7:::
    target02 | CHANGED | rc=0 >>
    root:*:19977:0:99999:7:::
    target03 | CHANGED | rc=0 >>
    root:*:19977:0:99999:7:::
    ```
16. **Quittez le Control Host et supprimez toutes les VM de l'atelier.**
    ```bash
    exit
    vagrant destroy -f
    ```