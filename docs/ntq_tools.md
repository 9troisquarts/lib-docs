---
sidebar_position: 1
title: 9tq toolbox
---

# Toolbox 9tq

La gem ntq_tools ajoute des fonctionnalités tel que le monitoring ou la génération de types graphql

## Installation

Ajoutez cette ligne à votre Gemfile :

```ruby
gem 'ntq_tools'
```

puis éxécuter la commande suivante :

```bash
bundle install
bundle exec rails generate ntq_tools:install
```

## Graphql

### Générer les types graphql a partir d'un model

```bash
bundle exec rails generate ntq_tools:graphql_scaffold MODEL_NAME
```

:::info Important
le schema.rb de votre application a besoin d'être à jour, il vous faut donc d'abord jouer les migrations.
la commande peut être relancée suite à une mise à jour du schema, mais les modifications apportées seront écrasées
::::