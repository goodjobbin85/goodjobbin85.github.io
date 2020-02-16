---
layout: post
title:      "API'S FOR BEGINNERS - A Gentle Introduction "
date:       2020-02-16 11:49:02 -0500
permalink:  apis_for_beginners_-_a_gentle_introduction
---


APIs can seem intimidating.  Understanding of them may seem out of reach for the average beginner.  Indeed, like everything else in programming, the amount of complexity involved when working with them can grow infinitely.  The purpose of this article is to shed some light on working with API's for the beginner, and to show that there is nothing to be feared. 

API stands for Application Programming Interface.  Data is at the core of programming, and API's help us retrieve data that either we would not normally have access to, or would otherwise need to scrape.  Most of the difficulty when dealing with API's lies in connecting the API to your application.  Given the somewhat ubiquitous nature of API's at the moment, it is a good bet that a given API, if often used and well maintained, will have solid documentation and consistent maintenance.  In this article well will be working with Ruby and the IEX exchange API to grab stock market information but first, we should understand the fundamental difference between using APIs and data scraping. 

Data scraping can be considered the brute force method for gathering require data.  Scraping works by inspecting the html code of a particular page, finding the data that you need by analysing the markup, and bringing that information into your program to work with.  This is the ideal method of data gathering when no API exists.  Its advantages lay in the fact that most pages are scrapable, and not all pages have APIs.  The problem comes when a website changes it's html markup, in which case all of your scraping code will break.  The programmer is at the mercy of the web page, and the scraped code can break at any moment, and the programmer can do nothing about it other than recode the scraper.  

With APIs, on the other hand, the website maintainer wants you to use their data. Often times, the programmer may even have to pay to get access to the data.  Due to this, the APIs are often well maintained and well documented.  Once in a while the code will be updated, but the amount of changes that will need to be made are far less than with scraping. 

The documentation for the IEX API for Ruby can be found at https://github.com/dblock/iex-ruby-client, and the steps for incorporating it into your code are simple. 

1. Install Ruby.  Install Ruby using your favorite package manager. I used RVM as I wanted to control the Ruby versions that my system had.  For this particular tutorial, I used Ruby version 2.6.1 
2. Create Gemfile.  Create a Gemfile in your program and require the iex gem by typing 

     ```
		 gem 'iex-ruby-client' 
		 ``` 
		 
3. Run 

     ``` 
		 bundle install 
		 ```  
		 
	   for your terminal to install iex on your local system 
4.  Get Token.  Go to "https://iexcloud.io/ " and sign up for an account to get your free token.  This token will give you      access to the data and will allow your code to retrieve stock information.  
5.  Include code snippet in your application. 

```
     client = IEX::Api::Client.new(
     publishable_token: 'token',
     endpoint: 'https://sandbox.iexapis.com/v1'
    ) 
```

This code snippet will allow you to instantiate a new client, store it in the client variable, and get the data.  Now we can do things like 

``` 
   quote = client.quote('MSFT')

   quote.latest_price # 90.165
   quote.change # 0.375
   quote.change_percent # 0.00418
   quote.change_percent_s # '+0.42%' 
``` 

The above code is an example from the ruby iex documentation found at "https://github.com/dblock/iex-ruby-client".

And that's it! We've connected to an API and were able to retrieve data from it.  There is so much more involved than this, but I hope that I was able to show that working with APIs should not be as intimidating as it seems. 

