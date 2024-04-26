---
sidebar_position: 2
title: Bonnes pratiques
---

## Convention de nommage de Describe

```ruby
RSpec.describe User, type: :model do

describe 'Methods' do
    # 
    describe '#full_name' do
      ...
    end

    describe '.admin?' do
      ...
    end

  end
end
```
