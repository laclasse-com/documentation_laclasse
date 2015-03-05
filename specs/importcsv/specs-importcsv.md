# Spécifications fonctionnelles 
# Intégration de l'import CSV des élèves et des profs dans l'annuaire ENT.

## objectif et contours fonctionnels
L'objectif est de développer une interface permettant le chagement et le traitement de fichiers CSV pour la création de comptes d'élèves et de profs dans les écoles primaires.
Cette interface est branchée au plus haut niveau de gestion des comptes, l'annuaire ENT, qui ensuite dispatche les comptes dans les 2 versions de l'ENT V2 et V3.
Les développements devront s'intégrer dans l'existant, en utilisant au maximum les fonctions, commandes et classes existantes. Il n'est pas demandé dans ce travail de refactorer l'ensemble de l'application, néanmoins des propositions de refactoring pourront être faites en fin de projet.

## L'existant
L'interface web de l'annuaire ENT est existante. Il faut donc intégrer la fonctionnalité CSV dans l'existant.
L'enchaînement des écrans est aussi déjà intégré, mais nécessitera d'être revue afin d'évoluer avec les developpements demandé. Aucun traitement n'a encore été ecrit, l'existant représente donc les mockups d'enchaînement des écrans.

## Structure du fichier issu de BASE_ELEVES
### Le fichier ELEVES

### Le fichiers PROFS

## Structure de l'annuaire ENT
### l'UID unique

## Correspondances des données

## Règles de gestion

## écrans d'import
### Zonning global

### Etape 1 : Choix du profil d'utilisateurs à importer
### Etape 2 : upload du fichier
### Etape 3 : prévisualisation des changements, application des règles de validation
### Etape 4 : Application des changements
### Etape 5 : Compte rendu d'activité

## Environnement technique 
### Client web
- HTML5 
- Twitter Bootstrap 
- JQuery

### Serveur
- Php 5.3

### Base de données
- Mysql
