# Viking Transport

Application web de gestion et de consultation d'un réseau de transport en commun en Normandie : consultation des lignes et des horaires, calcul d'itinéraires, réservation de billets avec système de fidélité, et espace d'administration.

Projet réalisé dans le cadre des **SAÉ 2.4, 2.5 et 2.6** à l'Université de Caen Normandie, par l'agence **DeviK**.

## Fonctionnalités

### Pour les visiteurs
- **Lignes du réseau** (`lignes.php`) : liste des lignes, avec filtre.
- **Horaires** (`horaires.php`) : horaires par arrêt, avec le détail du parcours au clic sur un passage.
- **Carte interactive** (`carte.php`) : visualisation du réseau et des lignes sur une carte Leaflet, avec panier multi-trajets.
- **Recherche d'itinéraire** (`trajet.php`) : trajet le plus rapide ou le plus court entre deux communes, avec correspondances (algorithme de Dijkstra, puis recherche des *k* meilleurs chemins) et calcul des horaires réels de passage.
- **Tarifs** (`tarifs.php`) : grille tarifaire indexée sur la distance et statuts de fidélité.
- **Réservation** (`reserver.php`) : réservation de voyages multi-segments, y compris sans compte.
- Pages d'information : `conditions.php`, `mentions.php`, `equipe_viking.php`, `equipe_devik.php`.

### Pour les clients connectés
- **Inscription / connexion** (`inscription.php`, `connexion.php`, `deconnexion.php`).
- **Profil** (`profil.php`) : informations personnelles, historique des réservations, points de fidélité.
- **Fidélité** : points gagnés à chaque voyage, points utilisables pour réduire le prix, grade selon les kilomètres cumulés.

### Pour les administrateurs
Accessible via le bouton « Admin » de la barre de navigation (réservé aux comptes présents dans la table `vik_administrateur`).
- **Tableau de bord** (`admin_dashboard.php`).
- **Gestion des lignes** (`admin_modif_ligne.php`) : ajout, modification, suppression des arrêts et des horaires.
- **Gestion des clients** (`admin_client.php`, `admin_modifier_client.php`, `action_supprimer_client.php`).
- **Statistiques** (`admin_stats.php`) : meilleurs clients, lignes les plus utilisées, réservations par période, clients fidèles, trajets populaires.

## Technologies

| Domaine | Outils |
|---|---|
| Back-end | PHP, PDO (driver `oci`) |
| Base de données | Oracle (schéma `vik_*`) |
| Front-end | HTML, Bootstrap 5.3, Bootstrap Icons, Leaflet |
| Versionnement | Git (Redmine de l'université) |

## Structure du projet

```
.
├── index.php                  # Page d'accueil
├── lignes.php, horaires.php   # Lignes et horaires
├── carte.php                  # Carte interactive
├── trajet.php                 # Calcul d'itinéraires
├── reserver.php               # Réservation
├── tarifs.php                 # Tarifs et fidélité
├── inscription.php, connexion.php, deconnexion.php, profil.php
├── admin_*.php, action_supprimer_client.php   # Espace administrateur
├── conditions.php, mentions.php, equipe_*.php # Pages d'information
├── template.php               # Modèle de page à copier pour créer une nouvelle page
├── includes/                  # head, topbar, footer, scripts JS communs
├── bdd/                       # Accès à la base de données
│   ├── env.php                # Paramètres de connexion Oracle
│   ├── BddConnexionUtils.php  # Ouverture de connexion et helpers PDO
│   ├── Bdd*Utils.php          # Requêtes par domaine (client, ligne, trajet, admin...)
│   ├── reserverutils.php      # Logique de réservation
│   ├── Inscription_utils.php  # Logique d'inscription
│   ├── testBdd.php            # Page de test de la connexion
│   └── ideeRequete.txt        # Brouillon des requêtes envisagées
├── css/, js/, assets/         # Styles, scripts et images
```

## Installation

### Prérequis
- PHP 8 ou supérieur avec l'extension **PDO_OCI** (Oracle Instant Client installé)
- Un accès à la base Oracle contenant les tables `vik_*`
- Un serveur web (Apache, Nginx) ou le serveur intégré de PHP

### Lancement

1. Récupérer le projet :
   ```bash
   git clone https://github.com/leocollet07/viking_transport
   cd viking_transport
   ```
2. Renseigner la connexion dans `bdd/env.php` :
   ```php
   $db_usernameOracle = "<utilisateur>";
   $db_passwordOracle = "<mot de passe>";
   $dbOracle = "oci:dbname=<hôte>:1521/<service>;charset=AL32UTF8";
   ```
3. Démarrer le serveur depuis la racine du projet :
   ```bash
   php -S localhost:8000
   ```
   Les chemins d'assets étant absolus (par exemple `/assets/equipe.png`), le site doit être servi à la **racine** du serveur.
4. Ouvrir <http://localhost:8000>.

Pour vérifier la connexion à la base, ouvrir `bdd/testBdd.php`.

> L'accès à la base de l'université nécessite d'être sur le réseau de l'université ou connecté à son VPN.

## Base de données

Tables utilisées par l'application :

| Table | Rôle |
|---|---|
| `vik_commune`, `vik_departement` | Communes (code INSEE) et départements |
| `vik_ligne` | Lignes du réseau |
| `vik_noeud` | Arrêts de chaque ligne : arrêt suivant, durée et distance jusqu'au suivant, horaires de passage |
| `vik_client`, `vik_type_client` | Clients, grade de fidélité, points |
| `vik_administrateur` | Courriels des comptes administrateurs |
| `vik_tarif` | Tranches tarifaires selon la distance |
| `vik_reservation`, `vik_etape` | Réservations et leurs étapes (segments par ligne) |

## Équipe

**Agence DeviK** (Caen) :

| Membre | Rôle |
|---|---|
| AHMADI Mohammad Elyas | Développement PHP et conception générale |
| CHAIGNON Nathan | Codéveloppement back-end |
| CHUQUET Anaël | Architecture logicielle et intégration front-end |
| COLLET Léo | Identité visuelle et design d'interface |
| CONSTANTIN Thomas | Architecture globale et back-end |
| GRENECHE Mathéo | Vues front-end et maquettes |
| GUILBERT Joan | Développement PHP et requêtes SQL complexes |
| PRENVEILLE Noé | Modélisation et conception de la base de données |
