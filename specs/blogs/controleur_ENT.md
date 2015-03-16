-----------------------------------------------------------------------------------------------------------------------------
Controleur du plugin ENT-WP-MANAGEMENT,
-----------------------------------------------------------------------------------------------------------------------------
Fonctionnement :
----------------
Dans tous les cas, l'authentification CAS et le script de provisionning de compte utilisateur et de blog passe avant ce contrôleur.
Il est positionné dans les premiers hooks exécutés par le moteur WP.
Les actions définies ici, passent dans une seconde phase.

Les paramètres utilisés en entrée
---------------------------------
```php
// Action
$_REQUEST['ENT_action'];

// Identifiant du blog dans l'annuaire
$_REQUEST['pblogid'];

// Nom du blog : en fait c'est le sous-domaine défini pour ce blog.
// ex :[monblogamoi].domain.com
$_REQUEST['blogname'];

// id Utilisateur : VAA61234
$_REQUEST['username'];

// Libellé ou description du blog.
$_REQUEST['blogdescription'];
```

Actions du controleur
---------------------

Pour certaines actions, on veut juste piloter Wordpress avec nos actions émanant de l'ENT
et ne rien afficher. La pluspart du temps il s'agit d'actions de mise à jours, ou de recherche :

- SUPPRIMER_BLOG : suppression d'un blog,
- USER_EXISTS : tester l'existence d'un user sur WP
- BLOG_EXISTS : tester l'existence d'un blog sur la plateforme.
- LOGOUT : Délogguer l'utilisateur de WP
- MODIFIER_PARAMS : Modifier les paramètres du blog dans Worpress et mettre à jour dans l'annuaire de l'ENT
- MIGRER_DATA : Migration des données de l'ancien blog (*** OBSOLETE ***)

Dans ces cas, on arrête tout traitement d'affichage. 

D'autres actions débouchent sur l'affichage du blog après avoir été effectuées "$mustDieAfterAction = false;" .
C'est le cas pour :
- IFRAME : l'encaspulation en iframe, qui ne fait qu'ajouter des filtres à WP, surcharger l modèle d'affichage avant de lancer l'execution du moteur et l'affichage du blog.
