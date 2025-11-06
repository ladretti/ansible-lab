##  1. Démarrer

```bash
vagrant up
```
```bash
vagrant ssh ansible
```
```bash
cd ansible/projets/ema/
ls
```
```
ansible.cfg  inventory
```

---

## 2. Tester l’accès aux hôtes

```bash
ansible all -m ping
```
rocky | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
debian | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
suse | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

---

## 3. Installer les paquets `tree`, `git` et `nmap` sur toutes les cibles

```bash
ansible all -m package -a "name=tree state=present"
ansible all -m package -a "name=git state=present"
ansible all -m package -a "name=nmap state=present"
```
1/
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}
2/
debian | CHANGED => {
    "cache_update_time": 1761245363,
    "cache_updated": false,
    "changed": true,
    "stderr": "",
    "stderr_lines": [],



---

## 4. Désinstaller ces paquets (`state=absent`)

```bash
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=nmap state=absent"
```

suse | SUCCESS => {
    "changed": false,
    "name": [
        "tree"
    ],
    "rc": 0,
    "state": "absent",
    "update_cache": false
}
rocky | CHANGED => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Removed: tree-1.8.0-10.el9.x86_64"
    ]
}
debian | SUCCESS => {
    "changed": false
}
suse | SUCCESS => {
    "changed": false,
    "name": [
        "git"
    ],
    "rc": 0,
    "state": "absent",
    "update_cache": false
}
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}
debian | SUCCESS => {
    "changed": false
}
suse | SUCCESS => {
    "changed": false,
    "name": [
        "nmap"
    ],
    "rc": 0,
    "state": "absent",
    "update_cache": false
}
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}

---

## 5. Copier le fichier `/etc/fstab` du Control Host vers tous les Target Hosts

```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
```
debian | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1762426127.2171657-6300-85566970519806/source",
    "state": "file",
    "uid": 0
}
rocky | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "secontext": "unconfined_u:object_r:user_home_t:s0",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1762426127.2239752-6299-69018434770240/source",
    "state": "file",
    "uid": 0
}
suse | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1762426127.2323022-6301-55015311382747/source",
    "state": "file",
    "uid": 0
}

---

## 6. Supprimer le fichier `/tmp/test3.txt` sur les Target Hosts

```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent"
```
debian | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
rocky | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
suse | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}

---

## 💽 7. Afficher l’espace utilisé par la partition principale

```bash
ansible all -m command -a "df -h /"
```
debian | CHANGED | rc=0 >>
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/debian--12--vg-root   62G  1.7G   57G   3% /
rocky | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        61G  2.0G   59G   4% /
suse | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        64G  2.3G   58G   4% /

---