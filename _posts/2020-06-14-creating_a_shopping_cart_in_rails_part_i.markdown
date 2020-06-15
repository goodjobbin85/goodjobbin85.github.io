---
layout: post
title:      "Creating A Shopping Cart In Rails: Part I "
date:       2020-06-15 03:11:09 +0000
permalink:  creating_a_shopping_cart_in_rails_part_i
---


This is the first in a two-part series in which we will learn how to create a shopping cart from scratch with Ruby on Rails.  Fundamental knowledge of Ruby and Ruby on Rails, in particular migrations, CRUD resources, different kinds of variables, blocks, etc is assumed, in order to quickly get to the primary focal points of the article. More advanced features of these topics, such as up and down methods for migrations, will be explained in detail.  In Part I, we will be setting up our models, views and controllers, as well as the activerecord relationships between these models. We will also incorporate sessions in order to keep track of our current cart. In Part II, we will run some more advanced migrations as well as introduce AJAX and ActionCable in order to provide realtime updates to our cart, and thus enhance the user experience. Also, we will incorporate security measures to ensure that only a cart that belongs to a particular user can be created or altered.

As previously stated, we will be creating a generalized shopping cart. For this article I will be be using the often frowned-upon scaffold generator(for the sake of brevity).  The general models we will be working with are Item, Cart, and LineItem. A LineItem is an Item in a Cart. LineItem belongs to Cart, LineItem belongs to Item, an Item has many LineItems and a Cart has many LineItems. 

```rails g scaffold Item```

```rails g scaffold Cart``` 

```rails db:migrate``` 

This sets up resources for Item and Product, then migrates the data to the database. 
Now, we want to create a method that will be used as a callback for Cart creation. We will store the cart.id in the session as 

`session[:cart_id]`


This will help us keep track of each cart that belongs to a particular user. 

```
def set_cart
    @cart = Cart.find(session[:cart_id])
rescue ActiveRecord::RecordNotFound
    @cart = Cart.create
    session[:cart_id] = @cart.id
end```

	
This method, which should be made private, will find a cart based on the value of session[:cart_id], and set it equal to @cart. If a cart is not found, it will be created.  Now, we need to create a LineItem resource that references item and cart: 

`rails g scaffold LineItem item:references cart:references` 

Now in order to add a particular item to a cart, we will add a button(which defaults to an HTTP POST request, as opposed to a link, which defaults to an HTTP GET request, to add a button to the cart next to each button on the items index page: 

`<%= button_to "Add To Cart", line_item_path(item_id: item.id) %>` 

And finally for the line_items create action, we create the line_item that references an item and is related to a cart: 

```
def create
    item = Item.find(params[:item_id])
    @line_item = @cart.add_item(item)

    respond_to do |format|
      if @line_item.save
        format.html { redirect_to @line_item.cart, notice: 'Line item was successfully created.' }
        format.json { render :show, status: :created, location: @line_item }
      else
        format.html { render :new }
        format.json { render json: @line_item.errors, status: :unprocessable_entity }
      end
    end
  end
```

Everything after the respond_to method is code that comes automatically with the scaffold generator. These are the first few steps in creating the necessary components of a shopping cart, and in the next section, we will cover advanced migrations, deleting carts, and writing methods that enhance the user experience.







