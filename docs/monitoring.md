---
sidebar_position: 5
title: Monitoring (Updown)
---

**Ojectif :** Chaque application en **production** et en **staging** a besoin d'être monitorée. \
Cela permet de réduire l'impact pour le client en intervenant au plus vite.

La mise en place se fait en 2 temps :
1. On installe et on configure la gem `ntq_tools`
2. On ajoute l'app dans [updown.io](http://www.updown.io)

_______________

### 1. Installer ntq_tools (> 0.7.2)

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

3. Paraméter les services :

    Par défaut, tous les services **sont activés** (database, redis et sidekiq). \
    Il est néanmoins possible de les désactiver manuellement :

    ```ruby title='config/initializers/ntq_tools.rb'
    NtqTools::Monitor.configure do |config|
      config.sidekiq = false
    end
    ```
:::danger Important
Vérifier que l'installation ou la mise à jour de `ntq_tools` n'entraîne pas de régressions liées aux dépendances.
:::

### 2. Définir un **login/mdp** dans les vars d'environnement

```
# .rbenv-vars
MONITORING_USERNAME=app-staging
MONITORING_PASSWORD=adY12*45!      # attention, pas de % cela perturbe updown
```

### 3. Tester l'url

- Le path est : **/ntq_tools/check.json**

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

### 4. Deploy en **staging / production**

- **Tester d'abord en staging** que tout fonctionne

- Ne pas oublier de **définir un login/mdp sur chaque environnement** et d'ajouter ces variables dans les bons fichiers (ou sur Scalingo).

### 5. Configurer l'url sur updown.io

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

  ```ruby
  login:mdp@<url-de-l-app>/ntq_tools/check.json
  # ex: https://foo:123@cfpro.test.com/ntq_tools/check.json
  ```

### 6. Metter à jour le tableau de suivi
[Voir le tableau](https://www.notion.so/9troisquarts/90aa5fc415b448dc9601907012124efe?v=6c7daf0f14a040ffa15897f8e758d7ed)
- Si l'app n'existe pas, la rajouter.
- Sélectionner le tag `Fait` dans la colonne "Updown".
