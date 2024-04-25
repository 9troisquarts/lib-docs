---
sidebar_position: 1
title: Mise en place
---

Chaque application en **production** et en **staging** a besoin d'être monitorée. \
Cela permet de réduire l'impact pour le client en intervenant au plus vite.

### 1/ Installer ntq_tools (> 0.7.2)

1. Ajouter la gem dans le Gemfile (groupe principal) ou vérifier la version de la gem :

   ```ruby
   # Gemfile
   gem 'ntq_tools'
   ```

2. Mount la gem dans les routes :

   ```ruby
   # routes.rb
    Rails.application.routes.draw do
      mount NtqTools::Engine => "/ntq_tools"
      ...
    end 
   ```
:::danger Important
Vérifier que l'installation ou la mise à jour de `ntq_tools` n'entraîne pas de régressions liées aux dépendances.
:::

### 2/ Supprimer la **gem Health-monitor-rails** si installée

Si la gem était installée, il faut tout SUPPRIMER ❌ 🗑️

  ```ruby
  # Gemfile
  gem 'health-monitor-rail'  => retirer cette ligne
  ```

  ```ruby
  # config/initializers/health_monitor_rails.rb => supprimer ce fichier
  ```

  ```ruby
  # routes.rb
    mount HealthMonitor::Engine, at: '/'. # supprimer cette ligne
  ```

### 3/ Définir un **login/mdp** dans les vars d'environnement

```
# .rbenv-vars
MONITORING_USERNAME=app-staging
MONITORING_PASSWORD=adY12*45!      # attention, pas de % cela perturbe updown
```

### 4/ Tester l'url **/ntq_tools/check.json**

- Renseigner le login/mdp (basic auth)

- Cela doit renvoyer un objet JSON :

  ```json
  {
    "status":"ok",
    "services_status":[
      {"name":"redis","status":"OK"},
      {"name":"database","status":"OK"},
      {"name":"sidekiq","status":"OK"}
    ]
  }
  ```

### 5/ Deploy en **staging / production**

- Tester d'abord en staging que tout fonctionne

- Ne pas oublier de définir un login/mdp pour chaque environnement et ajouter ces variables dans les bons fichiers (ou sur Scalingo).

### 6/ Configurer l'url sur updown.io

- Se logger à updown avec le SSO Gmail (compte support de 9tq)

- **Nomenclature** pour les alias :

  ```ruby
  <Client NOM DE L’APP - Environnement>
  # ex “Alive CAM - Staging”
  ```

- Monitorer la **production** ET le **staging**

- Pour la **production**, monitorer toutes les `5 min`

- Pour le **staging**, monitorer tous les `30 min` ou `1h`

- **Url** à renseigner :

  ```
  # ex: https://foo:123@cfpro.test.com/ntq_tools/check.json
  login:mdp@<url-de-l-app>/ntq_tools/check.json
  ```

### 7/ Update le **tableau de suivi**

- Si l'app n'existe pas, la rajouter
- Sélectionner le tag `Fait` \
https://www.notion.so/9troisquarts/90aa5fc415b448dc9601907012124efe?v=6c7daf0f14a040ffa15897f8e758d7ed
