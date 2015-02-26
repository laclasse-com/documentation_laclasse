# Spécifications fonctions
# Gestion de compétences

## Introduction
La validation de compétences au travers de référentiels normalisés est un outil pédagogique très employé par les collèges pour l'évaluation des élèves.
La tendance de ces dernières années et les prospectives pour à venir sont très claire. 
La disparition progressive des systèmes de notation classiques conduit les enseignants à utiliser les référentiels de compétences pour l'évaluation des niveaux des élèves.

La gestion des référentiel et de l'évaluation des élèves sont maintenant bien outillées. La version 2 de la classe.com comportait son propre module de validation des référentiels B2i, et Socle commun.
Dans cette nouvelle version, l'enjeu est de s'affranchir de la maintenance de ces outils afin d'intégrer des outils existants sous forme de services distants partageant la même authentification, ainsi que les données d'annuaire, lorsque cela est possible.

Les deux outils retenus pour l'intégration dans la version 3 sont :
- OBII (Outil en en ligne proposé par l'Education Nationale et hébergé et opéré par l'académie de Lyon).
- SACoche (logiciel opensource développé et maintenu par l'association Sesamath)
  Deux modes de fonctionnement sont possibles avec SACoche
    - mode hébergé chez Sesamath
    - mode déployé et herbergé dans les pools de serveurs de l'ENT Laclasse.com.
    
    C'est ce deuxième mode qui nous intéresse particulièrement.
    
## objectif

L'objectif est donc de proposer aux utilisateurs de laclasse.om v3, des accès aux applications OBII (via authentification académique) ou SACoche via authentification ENT, selon le choix pédagogique de leur établissement.
L'accès à ces applications se fera par le portail (où un carré "gestion de compétences" sera ajouté).

## Partage de l'authentification
Le logiciel SACoche est est écrit en PHP/Mysql, il fonctionne sur un serveur LAMP classique. Il possède déjà un client CAS lui permettant de se connecter à un serveur SSO d'ENT (ce qu'il fait déjà avec un certain nombre d'ENT, les configurations sso de plusieurs dizaines d'ENT sont enregistrées dans la configuration du logiciel).

Dans cette partie, il est demandé 
- d'interfacer SACoche avec le sso de laclasse.com.
- de se mettre en relation avec les responsables de SACoche afin qu'ils releasent la configuration sso LaclasseV3 de façon officielle dans leur application.

## Provisionning de données

## Intégration dans le portail
