# Règles de developpement

## Ruby 
Les règles de developpement ruby suivent les styleguide proposées par Github https://github.com/styleguide/ruby.

### Paramétrage des éditeurs
#### SublimeText 3
1. Installer les plugins suivants :
  - Rubocop
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
