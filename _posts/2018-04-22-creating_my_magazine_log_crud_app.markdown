---
layout: post
title:      "Creating my magazine log CRUD app"
date:       2018-04-22 05:16:35 +0000
permalink:  creating_my_magazine_log_crud_app
---


I used to be a rock chic :) I still like heavy music but I don’t listen to them as much as I used to. I used to go to concerts at least once a month, usually every weekend and bought all heavy metal magazines I could find. Many (many) years later, I have so many boxes of magazines taking up space so I was going to throw them away but… “one mans’ trash is another man’s treasure” right? It almost seems like there are buyers for anything on eBay. They even have a category called “Totally Bizarre Stuff” which has stuff like “Nature-Made/God created Heart in Watermelon” for $5000 or “Doritos Cheese Ball Tumor” for $35,000! (What!?) Anyway, I got derailed there but gist of it is that there are people who wants to buy my old magazines and I have so many of them, it’ll be useful for me to log my magazines to sell on eBay and track what I have. 

In creating my magazine log CRUD app, I wanted to keep it pretty simple. I only have two models, a user and a magazine. A user has many magazines and a magazine belongs_to a user. Just by adding has_many and belongs_to, we get methods like find_by and build. Cool!

```
class User < ActiveRecord::Base
  has_many :magazines

  has_secure_password
end

class Magazine < ActiveRecord::Base
  belongs_to :user
end

```

After creating my tables (ran migrations) and models, I made sure the associations are working in tux. I like that I can test them to make sure associations are good to go before setting up Model, View and Controller(MVC). My MVC pretty much followed the same pattern as the fwitter lab although I did use ActiveRecord validations in my user model which I could not use in fwitter lab (test was not passing). I included has_secure_password and a simple authentication was in effect using Bcrypt. 

```
class User < ActiveRecord::Base
  has_many :magazines

  has_secure_password

  validates :username, presence: true
  validates :username, uniqueness: true
  validates :password, presence: true

end

```

In The requirements, we had to make sure “Users can't modify content created by other users”
I used session[user_id] and checked to see if it matched the user’s id associated with the magazine to make sure it’s the person who created the magazine. I created helpers in application controller so I can refrain from repeating myself and so that the app doesn’t keep accessing the database every time it checks if the user is logged in or not.

```
  helpers do

    def logged_in?
      !!current_user
    end

    def current_user
      @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
    end

  end
	
	```
	
Routes were standard, just like fwitter lab and if and when it wasn’t working, I put binding.pry to figure out the problem. Actually the most tricky part was how to incorporate bootstrap into erb. I didn’t want to show a bare bone, text only app. I got the nav bar on top with 3 columns working but I still would need to go back and tweak for a more polished look. All in all, it wasn’t complicated to create and I had fun with CRUD. And I created something that is useful for me.


