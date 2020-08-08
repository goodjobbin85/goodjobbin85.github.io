---
layout: post
title:      "How to Set Up Rails As An API"
date:       2020-08-07 20:26:46 -0400
permalink:  how_to_set_up_rails_as_an_api
---


In this article, we will discuss how to set up Ruby on Rails to be used strictly as an API.  Why would someone want to do this?  By default, when a new project is created in Rails, view files are set up that will render html that relates to actions in a controller.  This is a good default. However, when we are interested in creating our own front end work separately from rails, we can set up our project as an API.  This will forego the creation of view files.  Instead, we will be rendering json objects from our controller, which will be sent over to be rendered and used on our front end.   To get started, let's create a new rails project, with one noticeable difference. 

   ``rails new banking-api --api`` 

This will notify Rails that this will be used as an API only.  Now, we can create a few resources to play with. 

   ```
	rails g scaffold user name email
  rails  g scaffold account number balance user:references
	```

In the User.rb file: 

   ``
	 has_many: accounts
	 `` 

And in the Account.rb file: 

   ``belongs_to :user`` 
	 
And now, for instance, we can update the Users Controller index action to render json objects of all users: 
   ```
	 def index
         users = User.all
         render json: users, include: [:name, :email]
     end
		 ```
		 
Finally, will have to to deal with CORS, which stands for  Cross-Origin Resource Sharing.  This helps us to prevent accessing resources that were not meant to be shared, only allowing us access to resources that we are allowed to access.  

First, uncomment the following line in your Gemfile and run bundle install: 

``# gem 'rack-cors'`` 

Finally, make your config/application.rb file look like the following: 

```
config.middleware.insert_before 0, Rack::Cors do
      allow do
        origins '*'
        resource '*',
          :headers => :any,
          :methods => [:get, :post, :delete, :put, :patch, :options, :head],
          :max_age => 0
      end
    end
```

And that's it! You have successfully set up your Rails project as an API!








