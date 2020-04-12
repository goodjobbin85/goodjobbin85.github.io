---
layout: post
title:      "Handling Sessions In Sinatra "
date:       2020-04-12 18:53:32 -0400
permalink:  handling_sessions_in_sinatra
---


Sessions and cookies are key concepts that must be understood in order to build secure web applications.  They are the tools that help us identify our users and restrict information access based on user identity.  This is a quick introduction into how to safely secure a web application using sessions.  

In this tutorial, we will be using sessions. Sessions are assigned a value(a user id, for instance), that value is hashed, and that session creates a cookie that can be persisted across multiple HTTP requests.  This way, for instance, we can sign up for a website, update information about ourself, navigate to the edit page, make changes, go back to our profile page, then log out, all operating under one session id.  

What IS a session though?  The following method is a post method that authenticates a user.  We've placed a binding.pry after the session has been created in order to inspect it's value 
	 
```
		post '/sessions' do
    @user = User.find_by(email: params[:email])
    if @user && @user.authenticate(params[:password])
      session[:user_id] = @user.id
      binding.pry
      redirect to "/users/#{@user.id}"
    else
      erb :'sessions/login'
    end
  end
```
		
If we type 
	
```
 	   session
``` 
	
now in our pry, we will have something like this 
	
     
   ```{"session_id"=>"fb66ef1a79676ab1f1ac77f1129e3aecab1fd7a7917d90b2d9e4c931b3bb06fb",    "csrf"=>"FerqGjEaLVESsv3W0Q1idoNtMTeJR9TBE70AJO5Q724=", "tracking"=>  {"HTTP_USER_AGENT"=>"e01a34d4cb71e5eac68842a652790690b98dce32"}, "user_id"=>31} ```
      
		 
We see that we have a hash digest, which represents a hashed version of the user id.  Great! How how do we incorporate this into our code? With the following line of code: 
		
```
  session[:user_id] = @user.id
```
		
As we can see from the login method above, we find a user based on the params of email, authenticate the value of the password with the "authenticate" method given to us by bcrypt, and if both of those conditions pass, we set the session[:user_id] equal to the id of the user.  Thats it! 
And how to we log out? Simple! 
		
```
		session.clear 
```
		
In fact, setting the session equal to nil will do the job as well.  Easy enough!




