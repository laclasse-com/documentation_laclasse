# Service d'état des applications
## description
Dans chaque application, une api donne l'état de celle-ci. Elle vérifie au minimum le service http, elle pourra vérifier l'état de ses services liés (backend mysql, Redis, etc...).

## Format de la réponse
```json
{ 
   "app_id":"PORTAIL",
   "app_version":"1.4.100",
   "rack_env":"production",
   "status":"OK",
   "reason":"L'application fonctionne."
}
```
- app_id : l'identifiant de l'application généralement trouvé dans la configuration de l'application.
- app_version : Donne la version de l'application actuellement déployée.
- rack_env : Donne l'environnement de dpéloiement ("test", "developpement", "production")
- status : état de l'application
  - OK : l'application fonctionne
  - KO : L'application ne fonctionne pas, la raison est précisée dans l'attribut 'reason'
- reason : précise la raison du dysfonctionnement (si le status est OK, reason a la valeur "L'application fonctionne.").
  - "Base de données inaccessible"
  - "Application en maintenance"
    
Si l'API ne répond pas, le client du portail se chargera de griser le carré de l'application et d'afficher le message "serveur inaccessible".

