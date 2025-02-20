# Installation d'un serveur DNS avec Windows Server
# Configuration du service DNS sur un Windows Server

## Prérequis

1. **Snapshot de la machine** : Prenez un snapshot de votre machine virtuelle pour pouvoir revenir en arrière en cas de mauvaise configuration.

## Installation du rôle DNS

1. **Ouvrir le Gestionnaire de serveur**.
2. **Cliquer sur "Ajouter des rôles et des fonctionnalités"**.
3. **Suivre les étapes jusqu'à la page "Sélectionner des rôles de serveur"**.
4. **Cocher la case "Serveur DNS"** et cliquer sur **"Suivant"** jusqu'à l'installation.

## Configuration du serveur DNS pour la zone `wilders.lan`

1. **Ouvrir le Gestionnaire DNS**.
2. **Cliquer avec le bouton droit sur "Zones de recherche directe"** et sélectionner **"Nouvelle zone"**.
3. Suivre l'Assistant Nouvelle zone :
    - **Type de zone :** Primaire
    - **Nom de la zone :** `wilders.lan`
    - **Fichier de zone :** Créer un nouveau fichier avec ce nom (db.wilders.lan.dns)

## Ajouter les enregistrements A et CNAME

1. **Ajouter l'enregistrement A pour le serveur** :
    - Dans le Gestionnaire DNS, développez la zone `wilders.lan`.
    - Cliquez avec le bouton droit sur la zone `wilders.lan` et sélectionnez **"Nouvel hôte (A ou AAAA)"**.
    - Entrez les informations suivantes :
        - **Nom :** `ns`
        - **Adresse IP :** `<IP_de_votre_serveur>`
    - Cliquez sur **"Ajouter un hôte"** puis sur **"OK"**.

2. **Ajouter l'enregistrement A pour la machine ayant une réservation IP fixe** :
    - Cliquez avec le bouton droit sur la zone `wilders.lan` et sélectionnez **"Nouvel hôte (A ou AAAA)"**.
    - Entrez les informations suivantes :
        - **Nom :** `machine`
        - **Adresse IP :** `172.20.0.10`
    - Cliquez sur **"Ajouter un hôte"** puis sur **"OK"**.

3. **Ajouter un alias CNAME pour le serveur** :
    - Cliquez avec le bouton droit sur la zone `wilders.lan` et sélectionnez **"Nouvel alias (CNAME)"**.
    - Entrez les informations suivantes :
        - **Nom :** `dns`
        - **Alias pointe vers :** `ns.wilders.lan`
    - Cliquez sur **"OK"**.

## Tests de la configuration DNS


### Vérifier la résolution de nom et la résolution inverse depuis le serveur

1. **Ouvrir une invite de commandes sur le serveur**.
2. **Tester la résolution de nom** :
    ```sh
    nslookup ns.wilders.lan
    nslookup machine.wilders.lan
    nslookup dns.wilders.lan
    ```
3. **Tester la résolution inverse** (remplacez `<IP_de_votre_serveur>` par l'adresse IP de votre serveur) :
    ```sh
    nslookup <IP_de_votre_serveur>
    nslookup 172.20.0.10
    ```
[![dns](https://github.com/fcisse-c/Installation_serveur_DNS/blob/main/dns.png)
### Vérifier la résolution de nom et la résolution inverse depuis une machine client

1. **Configurer la machine client pour utiliser le serveur DNS**.
2. **Ouvrir une invite de commandes sur la machine client**.
3. **Tester la résolution de nom** :
    ```sh
    nslookup ns.wilders.lan
    nslookup machine.wilders.lan
    nslookup dns.wilders.lan
    ```
    [![dns_server_machineClient](https://github.com/fcisse-c/Installation_serveur_DNS/blob/main/dns_server_machineClient.png)
4. **Tester la résolution inverse** (remplacez `<IP_de_votre_serveur>` par l'adresse IP de votre serveur) :
    ```sh
    nslookup <IP_de_votre_serveur>
    nslookup 172.20.0.10
    ```
[![test_dns_machineCliente](https://github.com/fcisse-c/Installation_serveur_DNS/blob/main/test_dns_machineCliente.png)


