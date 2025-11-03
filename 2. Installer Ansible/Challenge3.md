
# Challenge 1 : Préparer l'environnement

## Étapes à suivre :

1. **Démarrez la VM rocky**
   ```bash
   vagrant up rocky
   ```

2. **Connectez-vous à cette VM**
   ```bash
   vagrant ssh rocky
   ```

3. **Rafraîchissez les informations sur les paquets.**
   ```bash
   sudo dnf update -y
   ```

4. **Installation de python**
   ```bash
   sudo dnf install -y python3 python3-pip
   ```

5. **Installation de venv**
   ```bash
   sudo pip3 install virtualenv
   ```

6. **Création du venv**
   ```bash
    mkdir ~/ansible-env
    cd ~/ansible-env
    virtualenv venv
    source venv/bin/activate
   ```

7. **Installation de Ansible**
   ```bash
   pip install ansible
   ```

8. **Version de Ansible**
    ```bash
    ansible --version
    ```
    ```bash
    ansible [core 2.15.13]
        config file = None
        configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
        ansible python module location = /home/vagrant/ansible-env/venv/lib/python3.9/site-packages/ansible
        ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
        executable location = /home/vagrant/ansible-env/venv/bin/ansible
        python version = 3.9.21 (main, Aug 19 2025, 00:00:00) [GCC 11.5.0 20240719 (Red Hat 11.5.0-5)] (/home/vagrant/ansible-env/venv/bin/python)
        jinja version = 3.1.6
        libyaml = True
    ```