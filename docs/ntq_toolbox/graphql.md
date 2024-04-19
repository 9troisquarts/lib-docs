---
sidebar_position: 2
title: 'GraphQL'
---

### Générer les types graphql a partir d'un model

```bash
bundle exec rails generate ntq_tools:graphql_scaffold MODEL_NAME
```

:::info Important
le schema.rb de votre application a besoin d'être à jour, il vous faut donc d'abord jouer les migrations.
la commande peut être relancée suite à une mise à jour du schema, mais les modifications apportées seront écrasées
::::