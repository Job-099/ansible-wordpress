# Rôle Ansible WordPress

Ce rôle Ansible installe et configure WordPress avec Apache et MariaDB sur Ubuntu et Rocky Linux.

## Prérequis

- Ansible 2.9+
- Accès sudo sur les machines cibles
- Python MySQL library (`python3-pymysql` sur Ubuntu, `python3-PyMySQL` sur Rocky)

## Variables

### Variables obligatoires
```yaml
vault_wordpress_db_password: "motdepasse_securise"
vault_mysql_root_password: "motdepasse_root_securise"
```

### Variables optionnelles
```yaml
wordpress_db_name: wordpress                    # Nom de la base de données
wordpress_db_user: wpuser                      # Utilisateur de la base
wordpress_db_host: localhost                   # Hôte de la base
wordpress_admin_email: admin@localhost         # Email administrateur
wordpress_site_title: "Mon Site WordPress"     # Titre du site
wordpress_server_name: localhost               # Nom du serveur Apache
wordpress_document_root: /var/www/html         # Racine web
```

## Utilisation

### 1. Installation du rôle
```bash
ansible-galaxy install votre_nom.wordpress
```

### 2. Création du vault
```bash
ansible-vault create group_vars/all/vault.yml
```

Contenu du vault :
```yaml
vault_wordpress_db_password: "votre_mot_de_passe_db"
vault_mysql_root_password: "votre_mot_de_passe_root"
```

### 3. Playbook d'exemple
```yaml
---
- hosts: wordpress_servers
  become: yes
  roles:
    - role: votre_nom.wordpress
      vars:
        wordpress_server_name: "{{ inventory_hostname }}"
        wordpress_admin_email: admin@mondomaine.com
```

### 4. Exécution
```bash
ansible-playbook -i inventory playbook.yml --ask-vault-pass
```

## Handlers

- `restart web server` : Redémarre Apache/httpd
- `restart database` : Redémarre MariaDB
- `reload web server` : Recharge la configuration Apache

## Compatibilité

- Ubuntu 20.04, 22.04
- Rocky Linux 8, 9
- CentOS 8, 9

## Sécurité

- Mots de passe stockés dans Ansible Vault
- Permissions restrictives sur wp-config.php
- Suppression des utilisateurs MySQL anonymes
- Suppression de la base de test MySQL

## Licence

MIT

## Auteur

Votre nom (votre.email@example.com)