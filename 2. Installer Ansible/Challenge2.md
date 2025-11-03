# Challenge n°2 : Installer Ansible via un dépôt PPA

## Étapes à suivre :

### 1. **Ajouter le dépôt PPA d'Ansible**

```bash
sudo apt-add-repository ppa:ansible/ansible
```

### 2. **Mettre à jour les paquets**

```bash
sudo apt update
```

### 3. **Installer Ansible depuis le PPA**
```bash
sudo apt install -y ansible
```
### 4. **Vérifier la version d'Ansible installée**

```bash
ansible --version
```

```bash
ansible [core 2.17.14]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```