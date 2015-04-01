# Règles de developpement Ruby 
Les règles de developpement ruby suivent les styleguides proposées par Github https://github.com/styleguide/ruby.
L'avantage, c'est qu'il existe une gem qui permet de faire la vérification automatique du style du code.
## Paramétrage du projet
1. ajouter la gem rubocop
- Soit dans le gemfile, environnement  'test', soit vec la commande ```gem install rubocop```

2. Ajouter à la racine du projet, le fichier de paramétrage des règles ```.rubocop.yml``` avec la configuration suivante :
```yaml
Style/SpaceInsideBrackets:
  Description: 'No spaces after [ or before ].'
  StyleGuide: 'https://github.com/bbatsov/ruby-style-guide#no-spaces-braces'
  Enabled: false

Style/SpaceInsideHashLiteralBraces:
  Description: "Use spaces inside hash literal braces - or don't."
  StyleGuide: 'https://github.com/bbatsov/ruby-style-guide#spaces-operators'
  Enabled: false

Style/SpaceInsideParens:
  Description: 'No spaces after ( or before ).'
  StyleGuide: 'https://github.com/bbatsov/ruby-style-guide#no-spaces-braces'
  Enabled: false

Style/SpaceInsideRangeLiteral:
  Description: 'No spaces inside range literals.'
  StyleGuide: 'https://github.com/bbatsov/ruby-style-guide#no-space-inside-range-literals'
  Enabled: false
```

## Paramétrage des éditeurs
### SublimeText 3
1. Installer le plugin suivant :
  - *Rubocop*
  Positionnner dans la configuration utilisateur ```Preferences > Packages Settings > Rubocop > Settings - User```
  les paramètres suivants :
```json
{
  "rubocop_command": "/usr/local/bin/rubocop --auto-correct",
  "check_for_rvm": false,
  "rvm_auto_ruby_path":  "~/.rvm/bin/rvm-auto-ruby",
  "check_for_rbenv": false,
  "rbenv_path":  "/usr/local/bin/rbenv",
  "mark_issues_in_view": true,
  "show_auto_correct_warning": true,
  "rubocop_config_file": ""
}
```
La verification des règles est ensuite lancer sur ```ctrl+cmd+c```pour macOS.

### Emacs
1. Installer rubocop-emacs comme expliquer @ https://github.com/bbatsov/rubocop-emacs
2. Ajouter ceci à votre ~/.emacs
```lisp
  (add-hook 'ruby-mode-hook
	    (lambda ()
	      (add-hook 'before-save-hook 'rubocop-autocorrect-current-file nil 'local)))
```
