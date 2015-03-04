# Idée

L'idée est de mettre la configuration en base (REDIS ou Database) en mode clé -> valeur.
 - common.ma.mes.clé.
 - appli.ma.clé

Il serait alors possible de démarrer une apllication avec une
configuration minimale (e.g. juste l'adresse du serveur de confs REDIS,
ou autre), et de se configurer à partir de cette source.

Chaque appli accedrait à la configuration par le biais d'une API de laclasse-common.

On peut imaginer que, dans la conf minimal d'une appli, elle connaisse
son APP_ID laclasse et donc, que le module de configuration puisse aller
chercher les bonnes clefs en combinant :

- APP_ID
- CONF_BACKEND (e.g. "redis://redis.laclasse.lan:6379",
  "etcd://some.server", "mysql://user:pass@mysql.laclasse.lan",
"yaml:///path/to/file", ...)
- RACK_ENV (production, devel, ...)
- DOMAIN (e.g. laclasse.com)
- 
laclasse-commong pourrait alors chercher des clefs en combinant :

ENV['DOMAIN'].split('.').reverse.join('.') + ENV['RACK_ENV'] + ENV['APP_ID']

On pourrait donc aller chercher n'importe quelle clef de conf
facilement.

# Intérêt

L'intérêt d'un tel système est multiple.

## Indépendant du CM

À l'usage, on s'est rendus compte que c'était assez peu fiable de
déployer les confs (templates rendus par Ansible) depuis notre CM
actuel. En effet, on se retrouvait avec des différences entre l'état du
développement (nouvelles clefs de conf) et delui du CM, dans la mesure
ou les devs n'ont actuellement pas accès à l'inventaire du CM.

Les déploiements pètent donc régulièrement, en tous cas, presqu'à chaque
fois qu'une modif de conf est faite (multiplier par x2 quand on a pas
encore reporté le fix de la dev vers la prod).

Avec un tel système, les développeurs seront plus indépendants pour les
déploiements.

## Abstraction de l'implémentation de la gestion de la configuration
applicative

En fournissant une API pour accéder à sa propre conf, on s'abstrait de
l'implémentation de la gestion de cette conf.
On peut très bien implémenter des backends différents si nécessaire,
pour lire la conf depuis un fichier YAML, Json ou depuis Redis, etcd,
...

## Reconfiguration au runtime

En interrogeant systématiquement l'API de configuration (i.e. en évitant
de stocker une valeur de configuration dans une variable ou une
globale), on peut très bien imaginer reconfigurer les applications au
runtime, sans avoir à redémarrer les instances. Compte tenu des
performances des stores k/v, l'impact en termes de performance serait
minime.
On peut aussi imaginer un cache (court) de ces valeurs.

## Configuration depuis les applications

Certaines applis, lorsqu'elle démarrent, pourraient aussi fournir des
éléments de configuration aux autres applications. L'annuaire est un
exemple typique.
Cela implique d'anticiper dans le code des applications, et d'assurer un
fonctionnement même si ces données ne sont pas présentes (en présentant,
à minima, un message indiquant le problème).

# Interface

Un singleton semble être le pattern idéal pour implementer cet accès aux
confs.

Par exemple:

    class Configuration
      include Singleton

      def initialize
        # Instantiate backend
      end

      @@instance = Configuration.new

      def self.instance
        return @@instance
      end

      def []
        # return value from backend
      end

      def inject(uri)
        # Injecte une nouvelle configuration
      end
    end

    CONF = Configuration.new
    puts CONF[:secret_key]
    => "omgwtf"

# Alimentation des configurations

On peut imaginer que l'annuaire, pièce centrale et indispensable de
laclasse, serait chargé d'alimenter la base de données k/v avec les
bonnes configuration.

Charge à lui d'assurer:

- la vérification de la configuration actuelle
- l'invalidation des valeurs si elles sont erronnées
- l'insertion des bonnes valeurs

Il est nécessaire de versionner les confs (git sha1 par exemple), car
plusieurs instances de l'annuaire (ou de l'application en charge de
publier la bonne configuration) son susceptibles de fonctionner en //
(et de démarrer à des moments différents). Il faudrait éviter
d'invalider les configurations si elles sont bonnes.

Il y a donc un mécanisme à prévoir d'injection de configuration si les
données sont invalides (il faut donc, conserver dans ce système de conf
la version de la conf déployée).

Les confs seront gérées à priori sur github, mais doivent pouvoir
provenir d'une source locale (fichier Yaml, JSON, ...).

