---
layout: post
title:      "Redux 101: What is Redux, and Why Use It?"
date:       2020-10-11 19:50:38 +0000
permalink:  redux_101_what_is_redux_and_why_use_it
---


As we develop our frontend web applications, we will soon notice that we have an unruly mess of state, props and components.  Keeping track of this mess is the goal of Redux, and can be encapsulated by the idea of a "single source of truth".  This refers to the idea that all of an application's data should be stored in a single object, and any component that needs to refer to it can, simply by being connected to it. So how does it work? 


The general idea is to write a function that accepts a previous state as well as an action, then uses the action to produce a new state.  If we had a state of users, for instance, we could send in an action to fetch all users, show a single user, update a user count, delete user, or whatever action we need in our application.  The following is an example of such a function:  

```
function updateState(state, action){      
  switch (action.type) {
    case 'INCREASE_COUNT_1:
      return {count: state.count + 1}
    case 'DECREASE_COUNT'_BY_1:
      return {count: state.count - 1} 
		case INCREASE_COUNT_BY_10: 
		  return {count.state.count + 10} 
		case DECREASE_COUNT_BY_10: 
		  return {count.state.count - 10}
    default:
      return state;
  }
}
```  

And now, assuming state had a value of 10, we could call updateState to update state like so: 

```
updateState(state, {type: 'DECREASE_COUNT_BY_10'})
``` 

and we would have a return value of 0!  

These types of functions are called reducers because they reduce previous state an current action into a new state.  Of course these functions can get much more complicated, but this simplified function serves us well in showcase the power of redux.







