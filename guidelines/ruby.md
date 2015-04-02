# Règles de développement Ruby
Les règles de developpement ruby suivent les styleguides proposées par Github https://github.com/styleguide/ruby.
L'avantage, c'est qu'il existe une gem qui permet de faire la vérification automatique du style du code.
## Paramétrage du projet
1. ajouter la gem rubocop
- Soit dans le gemfile, environnement  'test', soit vec la commande ```gem install rubocop```

2. Ajouter à la racine du projet, le fichier de paramétrage des règles ```~/.rubocop.yml``` avec la configuration présente dans le fichier rubocop.yml fourni.

```yaml
Style/SpaceInsideBrackets:
  Enabled: false

Style/SpaceInsideHashLiteralBraces:
  Enabled: false

Style/SpaceInsideParens:
  Enabled: false

Style/SpaceInsideRangeLiteral:
  Enabled: false

Style/AsciiComments:
  Enabled: false

Metrics/ClassLength:
  Max: 300

Metrics/LineLength:
  Max: 130

Metrics/MethodLength:
  Max: 200
```

3. Guard
Si le projet utilise Guard, voici un exemple de Guardfile qui intégre *rubocop*
Ce paramétrage lance *rubocop* qui si lles tests passent.

```ruby
group :red_green_refactor, halt_on_fail: true do
	guard :rspec, cmd: 'bundle exec rspec', title: 'service-suivi-perso',
				  all_after_pass: false, all_on_start: false, failed_mode: :keep do
	  watch(%r{^spec/.+_spec\.rb$})
	  watch(%r{^lib/(.+)\.rb$})     { |m| "spec/lib/#{m[1]}_spec.rb" }
	  watch(%r{^api/(.+)\.rb$})     { |m| "spec/api/#{m[1]}_spec.rb" }
	  watch('spec/spec_helper.rb')  { 'spec' }
	end

	guard :rubocop, all_on_start: false do
		watch(%r{^api/(.+)\.rb})
		watch(%r{^lib/(.+)\.rb})
	end
end
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
