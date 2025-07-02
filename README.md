#  Déploiement automatisé de GLPI avec Ansible

##  Objectif

Ce projet a pour but de déployer automatiquement une plateforme GLPI (Gestion Libre de Parc Informatique) à l'aide d'Ansible, conformément aux bonnes pratiques ITIL, dans le cadre d'un projet pédagogique en administration systèmes et réseaux.

La plateforme est conçue pour :
- Inventorier automatiquement les postes clients grâce à GLPI Agent
- Gérer les incidents (tickets)
- Fournir une traçabilité complète des actions

##  Technologies utilisées

- **GLPI** 10.1.18 (version stable la plus récente)
- **GLPI Agent** ≥ 1.6.1
- **Ansible** ≥ 2.14
- **Apache2**, **MariaDB**, **PHP**
- **Ubuntu 22.04 / Debian 12**
- **OpenSSL** (certificats auto-signés)

##  Arborescence du dépôt

```plaintext
projet_glpi/
├── hosts.ini
├── vars.yml
├── roles/
│   ├── geerlingguy.apache/
│   ├── geerlingguy.mysql/
│   ├── geerlingguy.php/
│   └── glpi/
├── glpi-server.yml
└── README.md
```

##  Déploiement avec Ansible

### 1. Pré-requis

- Avoir Ansible installé 
- Cloner ce dépôt :  

  ```bash
  git clone https://github.com/Yaaaaaaaama/Projet_glpi.git
  ```

  ```bash
  cd projet_glpi
  ```

- Configurer votre inventaire inventory/hosts : (modifier avec vos infos )

#### Exemple:

[glpi_server]
glpi.local ansible_host=192.168.1.100 ansible_user=ubuntu

### 2. Lancer le déploiement

```bash
ansible-playbook -i inventory/hosts site.yml
```

Le playbook effectue :

- Installation de Apache, PHP, MariaDB

- Téléchargement et déploiement de GLPI

- Configuration SSL (certificats auto-signés)

- Création de la base de données glpi

## Client GLPI Agent

### Poste client : Ubuntu server 24.04

### 1. Installation du GLPI Agent :

```bash
wget https://github.com/glpi-project/glpi-agent/releases/download/1.6.1/glpi-agent_1.6.1-1_all.deb
```

```bash
sudo dpkg -i glpi-agent_1.6.1-1_all.deb
```

### 2. Configuration pour pointer vers le serveur GLPI :

```bash
sudo nano /etc/glpi-agent/glpi-agent.conf
```
#### Exemple:

server = http://glpi.local/front/inventory.php

### 3. Redémarrer l'agent :

```bash
sudo systemctl restart glpi-agent
```


