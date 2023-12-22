---
sidebar_position: 2
---

# Jobs Dashboard

La gem jobs dashboard permet de créer un dashboard des jobs sidekiq afin d'y afficher des infos tel que la durée...

## Installation

```ruby title="Gemfile"
# Use the latest version on master branch
gem 'jobs_dashboard', git: 'https://github.com/9troisquarts/jobs_dashboard'

# Use the last published version
gem 'jobs_dashboard'
```

And then execute:

```bash
bundle install
```

Générer les migrations 

```ruby
rails generate active_record:job_log_migration
```

Run the migration:

```ruby
rails db:migrate
```
## Utilisation

:::danger Important
Le log des jobs sidekiq fonctionne uniquement avec les jobs qui include Sidekiq::Worker, il n'est pas possible de l'utiliser avec ApplicationJob pour le moment
:::

```ruby title="initializers/sidekiq.rb"
require 'jobs_dashboard'

Sidekiq.configure_client do |config|
  JobsDashboard.configure_client_middleware config
end

Sidekiq.configure_server do |config|
  JobsDashboard.configure_server_middleware config
  JobsDashboard.configure_client_middleware config
end
```

Mount Engine to access web component:

```ruby title="routes.rb"
require 'jobs_dashboard/engine'

Rails.application.routes.draw do
  mount JobsDashboard::Engine, at: '/jobs_dashboard'
end
```

Accèder aux dashboard en vous dirigeant vers l'adresse `/jobs_dashboard` (ou tout autre url configurer dans le fichier routes.rb)

## Options

Des options spécifiques a jobs_dashboard peuvent être ajoutées aux jobs sidekiq de la manière suivantes :

```ruby
  sidekiq_options jobs_dashboard: { skip: true }
```

Voici une liste exhaustive des options disponible

| Clé  | Type          | description | default |
| :--------------- |:---------------:| :-----| :---- |
| skip | `boolean` | Permet d'ignorer un job et de ne pas le loger sur le dashboard | false |


## Ajout d'une ligne de log ou de métadonnées à un job

:::danger Important
Afin de pouvoir ajouter un de ces attributes, le job doit include le module JobsDashboard::Worker en plus de Sidekiq::Worker
::::

- Metadonnée : Les métadonnées d'un job sont les valeurs qui seront affiché dans l'index des jobs sur le dashboard, et afficher sur la partie droite de la page détaillé du job
- Ligne de log : Les lignes de logs sont affichées sur la page du job, elle permettent d'apporter des infos sur l'execution d'un job, comme le nombre de ligne importées ou le détail d'un mail envoyé à un utilisateurs

Dans les premières lignes du job, ajouter l'import de JobsDashboard::Worker tel que :
```ruby
include Sidekiq::Worker
include JobsDashboard::Worker
```

Dans le corps du job :
```ruby
# Ajout d'une ligne de log
add_job_log_line("123 lignes ont été importées")

# Ajout d'une métadonnée
add_job_metadata("lines_count", 123)
```

Autres exemples :

```ruby title="job.rb"
add_job_metadata("email_count", User.all.size)
add_job_metadata("mailer_type", "missions")
User.all.each do |user|
  add_log_line("Envoie d'un email avec X missions à l'utilisateur #{user.email}")
end
```

## Ajout d'un attribut custom commun a tous les jobs

Si vous souhaitez ajouter un champ a tous les jobs qui sera utilisé comme filtre / colonnes, suivez les instructions ci dessous :

```ruby
bundle exec rails g migration add_{{attribute_name}}_to_jobs_dashboard_job_logs
```

In an initializer

```ruby
JobsDashboard.setup do |config|
  config.additional_params = [
    {
      attribute_name: 'attribute_name',
      title: 'Title displayed on column header and more'
    }
  ]
end
```

In the job
```ruby
include JobsDashboard::Worker
set_job_parameter(attribute_name, value)
```

## HTTP Basic auth

L'ajout d'une authentification Basic est requise pour le bon fonctionnement de la GEM, pour la paramètrer, ajouter ces deux variables d'environnement dans votre fichier .rbenv-vars

```bash title=".rbenv-vars"
JOBS_DASHBOARD_AUTH_PASSWORD=mon_password
JOBS_DASHBOARD_AUTH_USERNAME=mon_user
```

:::danger Attention
  L'ajout doit aussi être fait sur l'environnement de production et staging
:::