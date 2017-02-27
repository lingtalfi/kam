Kam
============
2017-02-27




A framework to create php applications.




Intro
============
2017-02-27

Today, I start a new framework.

I'm excited because I believe this one will be the ultimate framework, or at least my ultimate.

I had long searching a long time for the secret of that perfect formulae that would unify all those concepts:
reusability, mvc, simplicity.

To me, simplicity is the most important. Simplicity is when the learning curve is short. You know that time is money,
so simplicity is your friend if you are a business man.

It's so easy to deviate from simplicity; like if you are seeking the truth, it's a constant effort that requires
good and judgment.

MVC, I don't like fancy words like that, so I dig in and found out that what I really wanted from MVC is the ability
to be able to change the theme of a web page quickly.

Css is already an effort into that direction, at the html code level.
But sometimes, you want to also change the html code itself, that's when css is not enough, and when you need
another system, which I will talk about in this page.


Re-usability. Hum, sorry, that's just a fancy word I made up, I don't have anything to say about it right now.
It's just a characteristic that emanates from good oop code.


So, this morning I was in my bed, torturing my mind to find a way to solve all those constraints in one framework.

I struggled a lot, and I had to go to the limit of my conscious mind. I went to the magic kingdom of dreams,
and finally, I found it: the alchemy that would unify those concepts, and relieve my mind.

It was really a quest, so now that I'm calm, I'm going to write my findings below, for me, and for anyone that
cares.




The overview
==================
2017-02-27

I don't have the overview yet, but I will put it here when all my ideas will be written down.

- Architecture



The architecture
====================
2017-02-27

It starts with a Request and an Application to handle it.
The Request is, well, you know what a Request is: it's when you ask something to somebody.

In this case, the somebody would be the Application.

So the Request is an object holding your request, and the Application the object that will respond to it.

There are different types of Application.
Two of them are:

- web application
- console application

Most of the time, we will use web applications (websites).
Some websites are composed of two websites: an admin website and a front websites, some others just have a front,
there are probably other websites types.

So, for a web application, a Request could be: show me the home page for instance.

The Request has a parameters property.

Thanks to the simplicity concept, that's all it has.

To handle the Request, the Application will delegate it's job to RequestListeners.

Imagine a circle. 

The Application throws the Request on that circle, and so the Request follows the circle path.

On that circle, we can put an any number of RequestListener, which would be represented by small dots on the circle.

A RequestListener basically does something with the Request.
Since the Request only has a parameter property, a RequestListener could set or get some parameters to/from the Request.

This system is simple, and yet powerful because we can add an infinity of RequestListeners if we want to.


From that system, there is an infinite number of ways to handle the Request.
One that I like, inspired from the symfony framework, is the following.

You have three RequestListeners:

- Router
- Controller Executer
- Response Displayer


Router
---------
The Router analyzes the Request and decides which Controller should handle it.

Alert: new word: Controller!

A Controller is just a fancy term, specific to this system (with three main RequestListeners), to represent 
an object responsible for handling a Request and return a corresponding Response.

The Response is also an object, internal to this system, which represents the Response of the Controller to the Request.

In the case of web applications, the Response is mostly used to hold the content of the page to display. 

Again, the Response is internal to this system and doesn't exist outside of the Application.
More on the Response before the end of this section.


So, back to the Router for now. The Router just decide which Controller to use, but actually doesn't execute it.

Instead, it adds the name of the Controller to use to the Request (via the parameters).

Then we have the Controller Executer. As you can guess, this RequestListener will see if a Controller name was 
set in the Request parameters, and if so, it will execute it.



The Response Displayer is the last step in this system, it converts the Response (if any) into a visible output
for the user.

The technique of first configuring the Response, and then display it gives us some flexibility.
For instance, we can decorate the Response before it is displayed.



The Controller returns a Response, that's another characteristic of the Controller I forgot to mention earlier,

















