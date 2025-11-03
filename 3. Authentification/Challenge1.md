
# Challenge 1 : Préparer l'environnement

## Étapes à suivre :

1. **Démarrez la VM rocky**
   ```bash
   sudo nano /etc/hosts
   ```
   ```
    # /etc/hosts
    127.0.0.1      localhost.localdomain  localhost
    192.168.56.10  control.sandbox.lan    control
    192.168.56.20  target01.sandbox.lan   target01   
    192.168.56.30  target02.sandbox.lan   target02
    192.168.56.40  target03.sandbox.lan   target03
    ```

2. **Ping les machines**
   ```bash
   for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
   ```
   ```
   PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.

    --- target01.sandbox.lan ping statistics ---
    1 packets transmitted, 1 received, 0% packet loss, time 0ms
    rtt min/avg/max/mdev = 0.421/0.421/0.421/0.000 ms
    PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.

    --- target02.sandbox.lan ping statistics ---
    1 packets transmitted, 1 received, 0% packet loss, time 0ms
    rtt min/avg/max/mdev = 0.255/0.255/0.255/0.000 ms
    PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.

    --- target03.sandbox.lan ping statistics ---
    1 packets transmitted, 1 received, 0% packet loss, time 0ms
    rtt min/avg/max/mdev = 0.298/0.298/0.298/0.000 ms
    ```

3. **Scan des machines**
   ```bash
   ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
   ```
   ```
    # target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
    # target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
    # target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
   ```

4. **Génération de clé SSH**
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
    SHA256:CeFD5ATqyI25KyJjyMG6RteOT/MHYoCkWbWq5QU/9TQ vagrant@control
    The key's randomart image is:
    +---[RSA 3072]----+
    |   .oo=          |
    | ... * .         |
    |oo+ . * E        |
    |+++= . = o       |
    |o++.*   S        |
    | B.o = .         |
    |*.+ +o. .        |
    |B+....o  .       |
    |B+  .. ..        |
    +----[SHA256]-----+
   ```

5. **Copy de la clé sur les targets**
   ```bash
   ssh-copy-id vagrant@target01
   ```
   ```
   /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    vagrant@target01's password: 

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'vagrant@target01'"
    and check to make sure that only the key(s) you wanted were added.
    ```
    ```bash
   ssh-copy-id vagrant@target02
   ```
   ```
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    vagrant@target02's password: 

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'vagrant@target02'"
    and check to make sure that only the key(s) you wanted were added.
    ```
    ```bash
   ssh-copy-id vagrant@target03
   ```
   ```
   /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    vagrant@target03's password: 

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'vagrant@target03'"
    and check to make sure that only the key(s) you wanted were added.
    ```

6. Test du ping via ansible
    ```bash
    ansible all -i target01,target02,target03 -m ping
    ```
    ```
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
