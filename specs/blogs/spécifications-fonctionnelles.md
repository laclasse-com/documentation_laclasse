# Module de Pilotage de la plateforme de blogs / Laclasse.com /V3.
# Spécifications fonctionnelles

## Introduction

Le module de Pilotage de la plateforme de blogs est un composant de l'ancienne version de laclasse.com. Il doit être ré-écrit pour la version 3.

Il sera ré-écrit à iso-fonctionnalité de la version existante.

## Description fonctionnelle

3 types de blogs :
* blog d'établissement
* blog de regroupement (classe, groupe d'élève, groupe libre)
* blog personnel

### Liste des blogs de l'utilisateur
(Où récupérer les blogs accessibles par un utilisateur ?)

### Création d'un blog
Seuls les profils PROF, ADM_ETB, CPE, PRINCIPAL peuvent créer un blog.
Pour créer un blog il faut définir :
* son type,
* son titre

### Suppression d'un blog
Seul l'utilisateur Administrator du blog ou un utilisateur Super Admin peut supprimer un blog.

1. L'utilisateur loggué possédant les prérequis ouvre l'application Blogs,
2. il passe en mode modification en cliquant l'icône flottante,
3. il retourne la case du blog qu'il souhaite supprimer et clique le bouton correspondant à l'action de suppression.

## Interface

L'application reprends le principe du damier des applications du portail de laclasse.com :
* un carré par blog accessible par l'utilisateur affiché sur un damier de 4✕4
* s'il y a plus de 16 blogs à afficher la liste scrolle verticalement
* l'utilisateur peut filter par type de blog, nom
* L'ajout, la modification et la suppression se fait en passant en mode « modification » en cliquant sur une icône ronde en surimpression. Dans ce mode « modification » les cases sont retournables pour éditer le nom du blog, la couleur de la case ainsi que différents paramètres à définir ; les cases sont ré-arrangeables ; les cases sont supprimable ce qui entraine la suppression du blog proprement dit. La création de blog se fait dans une popup (similaire à l'ajour d'application au portail.)

### Points d'entrée par type de profil

#### ADMIN
Devient super-administreur de tout les blogs, pas de création de blog.

#### PROF, ADM_ETB, CPE, PRINCIPAL
Deviennent administrateur de leur domaine si le domaine n'existe pas,
avec création de blog, sinon devient éditeur du blog existant.

#### PRINCIPAL
Si le blog est celui de son établissement : Devient administrateur de son domaine.
Pour tous les autres blogs, voir la règle ci dessus (profs, cpe, adm_etb).

#### ELEVE
Devient contributeur du blog existant dans le domaine, pas de création de blog.

#### PARENT
Devient souscripteur du blog existant, pas de création de blog.

## Cadre technique

### Développements

#### Serveur

  - Sinatra
  - Ruby

#### Backend

  - Il ne semble pas nécessaire de stocker des données localement, l'application ne faisant qu'ajouter une interface sur les APIs existantes.

#### Client

  - Angular JS

## Résumé des règles fonctionnelles importantes
