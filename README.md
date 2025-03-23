# Guide d'installation et configuration Ansible ↔ Netbox
## Introduction
Ce guide explique comment intégrer NetBox comme source d'inventaire dynamique pour Ansible. L'objectif final est de permettre à Ansible d'exploiter les données stockées dans NetBox pour automatiser la gestion des infrastructures de manière dynamique, sans avoir à renseigner manuellement les informations des dispositifs permettant ainsi d'éviter les erreurs causées par un inventaire statique non maintenu à jour.

## Installation de Netbox
Netbox peut être installé de plusieurs manières, ici c'est via Docker que Netbox sera installé : 
1. Cloner le dépôt Netbox
```bash
git clone https://github.com/netbox-community/netbox-docker.git
```
2. Lancer le conteneur docker Netbox
```bash
docker-compose  up -d
```
3. Accéder à Netbox
   - Interface web : http://localhost:8000
   - Identifiant : //////

## Installation d'Ansible 
Ansible sera installé ici dans un environnement virtuel python. Pour cela : 
1. Installer Python et venv
```bash
sudo apt update
sudo apt install python3 python3-venv -y
```
2. Créer l'environnement virtuel
```bash
python3 -m venv ansible-venv
source ansible-venv/bin/activate
```
3. Installer Ansible et le plugin Netbox
```bash
pip install --upgrade pip
pip install ansible pynetbox
ansible-galaxy collection install netbox.netbox
```
## Configuration d'Ansible pour utiliser Netbox comme inventaire dynamique
1. Créer le fichier permettant de configurer l'inventaire `netbox_inventory.yml`
2. Configurer l'inventaire dynamique
```bash
plugin: netbox.netbox.nb_inventory
api_endpoint: "http://localhost:8000"
token: "VOTRE_TOKEN_NETBOX"
validate_certs: false
```
3. Tester la connexion de notre hôte Ansible avec Netbox
```bash
ansible-inventory -i netbox_inventory.yml --list
```

## Conclusion
Avec cette configuration, Ansible peut désormais récupérer dynamiquement la liste des équipements inventorié dans votre Netbox et les utiliser dans ses playbooks.
