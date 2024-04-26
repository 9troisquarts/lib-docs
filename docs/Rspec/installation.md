---
sidebar_position: 1
title: Installation
---

Ajoutez cette ligne à votre Gemfile :

```ruby title="Gemfile"
gem 'rspec-rails'     # for rspec-core, rspec-expectations, rspec-mocks
```

puis éxécuter les commandes suivantes :

```bash
# Download and install
$ bundle install

$ rails generate rspec:install
      create  .rspec
      create  spec
      create  spec/spec_helper.rb
      create  spec/rails_helper.rb
```

Selon la nouvelle stratégie de versionnement de RSpec Rails, il faut utiliser :

- rspec-rails 6.x pour Rails 6.1 ou 7.x.
- rspec-rails 5.x pour Rails 5.2 ou 6.x.
- rspec-rails 4.x pour Rails à partir de 5.x ou 6.x.
- rspec-rails 3.x pour Rails antérieur à 5.0.
- rspec-rails 1.x pour Rails 2.x.

## Premier test

Voici un exemple de test basique qui montre la syntaxe attendue pour vos projets. 

Imaginons que nous ayons un model User qui contient cette méthode : 

```ruby
def full_name
  full_name_arr = []
  full_name_arr.push first_name, last_name
  full_name_arr.compact.join(' ')
end
```

Voici comment nous pourrions tester cette méthode ```full_name``` :

```ruby
require 'rails_helper'

RSpec.describe User, type: :model do

subject(:user) { FactoryBot.create(:user, first_name: 'Tony', last_name: 'Truand') }

describe 'Methods' do
    describe '#full_name' do

      context 'when first_name and last_name are present' do
        it 'returns the user full name' do
          expect(user.full_name).to eq('Tony Truand')
        end
      end

      context 'when last_name is nil' do
        it 'returns the user full name without his last name' do
          expect(user.full_name).to eq('Tony')
        end
      end

    end
  end
end
```

Voici les points à noter :

- Utilisation de ```subject``` pour définir un objet commun qui sera testé dans plusieurs exemples (ici une instance de la classe User)
- Utilisation de ```describe```  pour grouper un ensemble de tests (ici pour la méthode full_name)
- Utilisation de ```context```  pour grouper un ensemble de tests dans un état précis (ici l'état ou last_name est présent/ne l'est pas)

## Exécuter les tests

### Tous les tests

```bash
bundle exec rspec
```

### Les tests d'un dossier spécifique

```bash
bundle exec rspec spec/models
```

### Les tests d'un fichier spécifique

```bash
bundle exec rspec spec/models/user_spec.rb
```

### Un seul test dans un fichier spécifique

```bash
bundle exec rspec spec/models/user_spec.rb:8
```