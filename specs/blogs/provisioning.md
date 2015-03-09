WORDPRESS ROLES
---------------
* Super Admin - Someone with access to the blog network administration features
controlling the entire network (See Create a Network).
* Administrator - Somebody who has access to all the administration features
* Editor - Somebody who can publish and manage posts and pages as well as manage
other users' posts, etc.
* Author - Somebody who can publish and manage their own posts
* Contributor - Somebody who can write and manage their posts but not publish them
* Subscriber - Somebody who can only manage their profile

Les profils Laclasse.com suivants sont gérés :
----------------------------------------------
- ADMIN : Devient super-administreur de tout les blogs, pas de création de blog.
- PROF,
- ADM_ETB, CPE, PRINCIPAL : Deviennent administrateur de leur domaine si le domaine n'existe pas,
avec création de blog, sinon devient éditeur du blog existant.
- PRINCIPAL : Si le blog est celui de son établissement : Devient administrateur de son domaine.
Pour tous les autres blogs, voir la règle ci dessus (profs, cpe, adm_etb).
- ELEVE : Devient contributeur du blog existant dans le domaine, pas de création de blog.
- PARENT : Devient souscripteur du blog existant, pas de création de blog.

Les paramètres d'entrée sont les suivants :
--------------------------------------------
- blogname : Nom du sous-domaine de blog à créer. A ce nom est ajouté ".blogs.laclasse.com"
- blogtype : Type de blog :
  - CLS : Blogs de classes.
  - ETB : BLogs d'établissement ( page d'établissement).
  - GRP : Blogs de groupe d'élèves
  - ENV : Blogs de groupes de travail.
  - USR : Blogs personnels.
- debug : mode traçage des actions (qui sont réalisées, mais sans redirection finale).
- action : Par défaut l'action est le provisioning (création de user, de blog et redirection).

SUPPRIMER_BLOG : suppression physique du blog.

- idAncienBlogEnt : identifiant dans l'ENT de l'ancien blog à migrer vers WordPress.
Utilisé dans la création de l'article par défaut.

Règle particulière pour le super Admin :
S'il arrive sur le wpmu sans les paramètres blogname et blogtype il est qd
même routé sur l'administration de tout les blogs.
