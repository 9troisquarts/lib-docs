---
sidebar_position: 1
title: Backups manager
---

# Backups manager

Afin de faciliter la récupération et la restauration de base de données de *STAGING* sur les environnement de dev, une gem ruby est mis à disposition

## Installation

```bash
gem install scalingo_backups_manager
```

Ou dans le gemfile de votre projet

```ruby
# Ou dans un gemfile
gem 'scalingo_backups_manager'
bundle install
```

Ajouter une variable d'environnement ```SCALINGO_API_TOKEN``` avec comme valeur une clé API valide récupérable sur vos [paramétres utilisateur scalingo](https://dashboard.scalingo.com/account/keys)

Puis éxécuter la commande suivante :

## Initialisation et paramètrage du projet scalingo

```bash
bundle exec scalingo_backups_manager install
```

Il vous sera demandé de choisir un des projets auquel vous avez accés sur scalingo en entrant le numéro affiché sur votre terminal, puis il vous sera demandé les addons à configurer, en général, uniquement la base de données du projet (Postgresql ou Mysql)

## Commande

### Télécharger et restaurer le dernier backup
```bash
bundle exec scalingo_backups_manager download
```

### Télécharger le dernier backup

```bash
bundle exec scalingo_backups_manager restore
```