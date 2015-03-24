# Service d'état des applications
## description
Dans chaque application, une api donne l'état de celle-ci. Elle vérifie au minimum le service http, elle pourra vérifier l'état de ses services liés (backend mysql, Redis, etc...).

## Format de la réponse
```json
{ 
   "app_id" : "DOCS",
   "status" : "OK",
   "reason" : ""
}
```
- app_id : l'identifiant de l'application généralement trouvé dans la configuration de l'application.
- status : état de l'application
  - OK : l'application fonctionne
  - KO : L'application ne fonctionne pas, la raison est précisée dans l'attribut 'reason'
- reason : précise la raison du dysfonctionnement (si le status est OK, l'attribut reason est vide).
  - "Base de données inaccessible"
  - "Application en maintenance"
    
Si l'API ne répond pas, le client du portail se chargera de griser le carré de l'application et d'afficher le message "serveur inaccessible".

