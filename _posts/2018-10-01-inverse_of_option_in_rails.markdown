---
layout: post
title:      "'inverse_of' option in Rails"
date:       2018-10-01 18:09:20 -0400
permalink:  inverse_of_option_in_rails
---


For my rails project, I created FosterApp for parents who foster kittens. It only has basic features but would like to add more in the future. I started creating the models. 

```ruby
class Litter < ApplicationRecord
  has_many :kittens, dependent: :destroy
  
  validates :name, presence: true, uniqueness: true

  accepts_nested_attributes_for :kittens,
                                allow_destroy: true,
                                reject_if: proc {|att| att['name'].blank? }

```

```ruby
class Kitten < ApplicationRecord
  belongs_to :litter, optional: true
end


```

A basic standard relations. Litter `has_many` kittens and kittens `belongs_to` a litter. Since kittens are the child of a litter and kittens won't exist without a parent, I wanted to have a form where I create a litter and kittens at the same time so I used `accepts_nested_attributes_for`. Time to test relations in rails console.

First, create a litter.
```
2.3.3 :004 > Litter.create(litter_name: 'V18')
   (0.1ms)  begin transaction
  Litter Create (31.3ms)  INSERT INTO "litters" ("litter_name", "created_at", "updated_at") VALUES (?, ?, ?)  [["litter_name", "V18"], ["created_at", "2018-09-14 17:27:58.417648"], ["updated_at", "2018-09-14 17:27:58.417648"]]
   (58.7ms)  commit transaction
 => #<Litter id: 1, litter_name: "V18", foster_start_date: nil, foster_end_date: nil, with_mom: false, mom_name: nil, created_at: "2018-09-14 17:27:58", updated_at: "2018-09-14 17:27:58"> 
```

Then create a kitten.
```
2.3.3 :013 > kit1 = Kitten.create(name: "Velvet")
   (0.1ms)  begin transaction
   (0.1ms)  rollback transaction
 => #<Kitten id: nil, user_id: nil, litter_id: nil, name: "Velvet", sex: nil, color: nil, created_at: nil, updated_at: nil> 
```

It kept rolling back transactions and I was’t able to create kittens at all. After googling a ton, I found out I needed `inverse_of` on the associations.

```ruby
class Litter < ApplicationRecord
  has_many :kittens, dependent: :destroy, inverse_of: :litter
  
  validates :name, presence: true, uniqueness: true

  accepts_nested_attributes_for :kittens,
                                allow_destroy: true,
                                reject_if: proc {|att| att['name'].blank? }

```

```ruby
class Kitten < ApplicationRecord
  belongs_to :litter, optional: true, inverse_of: :kittens
end


```

Now I can create kittens. 
```
2.3.3 :001 > kit1 = Kitten.create(name: "Velvet")
   (0.1ms)  begin transaction
  Kitten Create (0.5ms)  INSERT INTO "kittens" ("name", "created_at", "updated_at") VALUES (?, ?, ?)  [["name", "Velvet"], ["created_at", "2018-09-14 19:11:02.669758"], ["updated_at", "2018-09-14 19:11:02.669758"]]
   (28.0ms)  commit transaction
 => #<Kitten id: 1, user_id: nil, litter_id: nil, name: "Velvet", sex: nil, color: nil, created_at: "2018-09-14 19:11:02", updated_at: "2018-09-14 19:11:02"> 
```

By including the`inverse_of` option in the `has_many` association declaration, Active Record will now recognize the bi-directional association. This option is usually not required in Rails 5  but when using `accepts_nested_attributes_for` with a `has_many` relation, it is necessary.
