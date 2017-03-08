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
- Services
- Modules and hooks
- MVC
- Conclusion



Architecture
====================
2017-02-27

It starts with a Request and an Application to handle it.
The Request is, well, you know what a Request is: it's when you ask something to somebody.

In this case, the somebody would be the Application, and the something would be the Request.

So the Request is an object holding your request, and the Application is the object that will respond to it.

There are different types of Application.
Two of them are:

- web application
- console application

Most of the time, we will use web applications (websites).
Some websites are composed of two websites: an admin website and a front websites, some others have just a front,
and there are probably other types of website.

So, for a web application, a Request could be: "Show me the home page" (for instance).

The Request has a **parameters** property.

Thanks to my love of simplicity, that's all it has.

To handle the Request, the Application will delegate it's job to RequestListeners.

Imagine a circle. 

The Application throws the Request on that circle, and asks for RequestListeners to do the job.

So the Request follows the circle path.

On that circle, we can put an any number of RequestListener, which would be represented by small dots on the circle.

A RequestListener basically is an object that does something with the Request.
Since the Request only has a parameter property, a RequestListener can set/get some parameters to/from the Request.

This system is simple, and yet powerful because we can add an infinity of RequestListeners if we want to.


Using this system, there is an infinite number of ways to handle the Request.
One that I like, inspired from the symfony framework, is the following.

You have three RequestListeners:

- Router - choosing the controller (this term is explained below)
- Controller Executer - executing the controller and returning a response (this term is explained below)
- Response Executer - interpret the response



In a nutshell, here is what happens: the Router first analyzes the Request and chooses a Controller, but doesn't execute it.

It passes the Controller to the Controller Executer, via the **Request.parameters**.
The Controller Executer executes the Controller, which returns a Response.
The Controller Executer passes the Response to the Response Executer, via the Request.parameters.

The Response Executer executes the Response, producing the expected result for the end user.



[![kam-architecture.jpg](https://s19.postimg.org/wgu809cv7/kam_architecture.jpg)](https://postimg.org/image/6lahh2b1b/)




Router
---------
The Router analyzes the Request and decides which Controller should handle the Request.

Alert: new word: Controller!

A Controller is just an object --specific to this system with three main RequestListeners-- 
responsible for handling a Request and return a corresponding Response.

Alert: new word: Response!

The Response is the object --specific to this system with three main RequestListeners-- responsible for holding the 
response to the Request.

In the case of web applications, the Response is mostly used to hold the content of the page to display.
 
Again, the Response is internal to this system and doesn't exist outside of this system's Application.

So, back to the Router for now. The Router decides which Controller to use, but doesn't execute it.
Instead, it adds the name of the Controller to use to the Request (via the Request parameters property).


The Controller Executer
---------------------------
Then we have the Controller Executer. As you can guess, this RequestListener will execute the Controller, which name was 
previously set by the Router. 
The Controller Executer doesn't do anything special if a Controller is not found in the Request parameters property.

The Controller returns a Response, and the Controller Executer binds the Response to the Request.

If the Controller doesn't return a proper Response, the Controller Executer complains by throwing an exception.



The Response Executer
-------------------------
The Response Executer is the last step in this system, it converts the Response (if any) into a tangible output
for the user.

For instance, displaying a web page.




Comments
------------

The technique of first configuring the Response, and then display it gives us some flexibility.
For instance, we can decorate the Response before it is displayed.

So, as you can see, the technique of separating the choosing of an object to use and actually using it gives us some flexibility.
We used the same technique for the Controller: the Router first chooses the Controller to use, and then the Controller Executer 
executes it.

This system with three RequestListeners is just an example of how we can handle a website with just three RequestListeners.

By adding/retrieving/replacing those RequestListeners, one can create an adapted structure for any application.







Services
===========
2017-02-27

Once the architecture is chosen, we also need to choose how the services of the application will be used.

For instance, if we have a pdo service, which provides a pdo wrapper, how will we access that services
from within our application code?

In symfony, from memory, we have a container, which instantiates services on demand.
I like the idea of a centralized container, however (if I remember correctly) there is a problem 
with the symfony container: it loads all the parameters at once, even if you don't use them.

So, although the services are lazily instantiated, the parameters are not.

I prefer to use a container which only consumes what is strictly needed: the instantiation of the service AND the parameters
should be created only when we need them.

There is a cost for that, but I'm willing to pay it.

The solution is to create the container by hand, and incorporate the parameters withing the method that instantiate the service
for the first time.

Also, a service container should be accessible from anywhere, and so I suggest a short class name.

My solution is to create a class named X.

X is the service container class. It contains only public static methods, each of which being a service.

With X, this code is possible:

```php
<?php 

$items = X::pdo()->fetchAll("select * from users");

```

As I said, the pdo method should instantiate the parameters only on the first call to the pdo method.

But there is a little problem here: you might have different environments: for instance you might have a database password
for the development environment, and a different password for the production environment.

Fortunately, I believe that this is the only problem we have to face.
Assuming that this is the only problem, we can solve it by using a convention.

Basically, the environment of the Application is a paradigm, and we can say that there are two main environments:

- dev - the development environment
- prod - the production environment


And therefore, we can create an object responsible for holding those environment names, and every developer should
be aware of those environment, and differentiate the configuration of her service based on those environment names (if necessary). 



I believe that giving a parameters property to the Application, and creating an environment parameter is a good idea.
Here is a possible implementation, using a singleton Application.


```php
Application::inst()->set("environment", "dev");
Application::inst()->get("environment"); // dev
```

With such a mechanism, the **X::pdo** service could look like this.


```php
class X {
    
    private static $services = [];
    
    public static function pdo(){
        if(false === array_key_exists("pdo", self::$services){
            // instantiate the service for the first time
            
            // the db parameters depends on the environment
            if('dev' === Application::inst()->get('environment')){
                $params = [
                    "host" => "localhost",
                    "user" => "root",
                    "pass" => "root",
                    "db" => "mylocaldb",
                    "port" => "3306",
                ];
            }
            else{
                // assuming prod environment
                $params = [
                    "host" => "localhost",
                    "user" => "zora",
                    "pass" => "g540TJ4",
                    "db" => "zora_db",
                    "port" => "3306",
                ];            
            }
            
            self::$services['pdo'] = SomeObject::createPdoInstance($params);
            
        }
        return self::$services['pdo'];
    }

}
```

With this technique, you can even add as many environments as you want (although we generally only need 2).

The important point here is to understand that the service container belongs to the application: different applications
will use different services containers.

Creating the container by hand gives you the ability to consume exactly what you need in terms of parameters, but it also
gives you a transparent service container, that you can just use as any other class.

Finally, when we will talk about modules later, you'll see that modules can automatically add the services they use (instead of manually
creating the services).




Modules and hooks
===========
2017-02-27 --> 2017-03-08

Maybe it's a little early to talk about modules, but since I've talked about them in the previous section,
I now have them in mind, so let's see what I have to say about them.

A classical definition that I used before was: a module is a piece of code that brings functionality
to your application.

Well, now I think that this definition is at least misleading, and at most wrong.

Modules and hooks are like boats and ports.

The boat transports merchandise to a port.

It's easy to forget about the port, but the port is as important as the boat (and the merchandise) itself.

If you don't have a port to land, then your boat is useless.

Actually, to deliver the merchandise at a port is the reason why we need the boats in the first place.

The module is just a medium.

My ideas of a module come from the following use case, where we want to create an admin website, 
like the wordpress admin website.

As the admin developer, you decide to create a left menu, and you want to allow other developers to feed that
menu with their own items.

So, you create a hook (more on hooks later), to which module can subscribe.

Then when the hook is executed, modules can bring their own menu items, and so the menu is composed of the items 
added by the subscribing modules.


I believe this example is the main reason why we need modules in the first place.

Basically, when you want other developers to hook into your application, you create a hook for them,
and they provide modules which subscribe to that hook, it's just a convention.


That being said, there are multiple hook implementations.
And I have some thoughts about implementation as well.

When I talked about the service container, I didn't like to create extra parameters that wouldn't be used.

The same logic applies here, and I don't want that a hook asks every module if it has subscribed to the service or not.
Rather, I prefer to use a hardcoded method which only calls the modules concerned with the hook.

So, here is the kind of implementation I don't like: register all modules at the beginning, in case they will be executed:


```php
foreach(SomeApplicationHelper::getModules() as $module ){
    AllApplicationHooksRegisterer($module)
}

```


Rather, I prefer the hardcoded approach:


```php
class MyLeftMenuHelper {

    public static function getLeftMenuElements(){
        $elements = [];
        ModuleOne::feedLeftMenuElements($elements);
        ModuleTwo::feedLeftMenuElements($elements);
        return $elements;
    }
}
```

It might be a little harder to automate at first, but the benefit is that we only code what is strictly required.
It's about code optimization and simplicity of conception.


I'm giving just an implementation hint here (that I discovered in nullos admin, look for SAAS if you are interested in) 
about a possible implementation of that hardcoded system: if you remove every php code,
except for the module calls, one per line, it's easier to automate:


```php
class MyLeftMenuHelper {

    public static function feedLeftMenuElements(array &$elements){
        ModuleOne::feedLeftMenuElements($elements);
        ModuleTwo::feedLeftMenuElements($elements);
    }
}
```

As you can see, with conventions, we can circumvent a lot of implementation problems.
But this document is not bound to any implementation in particular, that was just a tip that came out from my memory,
and I thought it was worth giving it away.






MVC
============
2017-02-27

This section is the very reason why I was struggling with the creation of that unified framework, it was the missing
piece of my personal researches.

What do we have under that fancy term: MVC.

To me, mvc is just about implementing the idea of being able to switch theme.

For instance, you create an e-commerce website.

To make it more appealing at the business level, you want to add this line in the features section:

- can change the theme in one click

The problem is: to change a theme in one click, you need to put a lot of efforts in the conception.

I'm not talking about just changing the css theme, although I can see how one could get away with that.

No, I'm talking one layer deeper: being able to change the html code as well.

I was trying to figure out who was responsible to display what, and then I ran into the javascript code,
and a long nightmare began: who is responsible for displaying the javascript code when a dialog opens, and what
if you use jquery ui, and what if you don't, and.... aaaarrgh kill me now please...

Finally, I found an answer that satisfied me, and here it is.




Template
----------------
2017-02-28

I want to start with the template.
What's a template?

The goal of the template is to display data.

A template is a way to present data.

In its simplest form, a template is a string with placeholders.

For instance, this is a template:

```txt
Hello {name}, how are you?
```

In this case, there is only one placeholder (name), which I wrapped within curly braces.
Placeholders might be called tags, and have different forms, but basically that's a template.

That's a dumb template though: it has no brain.

If we want, we can create smart templates, that is: a template that exposes an api, for instance

- if (condition)
- loops (iterate over an item)
- ...inheritance stuff, or whatever, sky is the limit


Smart templates might be written in any language. If you don't have any particular constraint,
you can use php itself, because, why reinvent the wheel?

Or you could create your own language, because, why not reinvent the wheel?

That doesn't really matter. 

So, okay for the template refresher.

However, we didn't discuss the WHERE part yet. WHERE does the data come from?

If I take my first example again:

```txt
Hello {name}, how are you?
```

Where does "name" come from?


Spoiler alert-- 

The answer is that it comes from the Controller.
Because if you remember well, the Controller is responsible for displaying the page (in a web application for instance),
so this means it controls every aspect of the page.




Data, variables and model
-------------------
2017-02-28

Let's imagine we're a boss, and we're telling two persons to create a template which should display the 
name of the person.


Paul will create this template:

```php
Hello {name}, how are you?
```

And Alice will create this template:

```php
Hello {person.name}, how are you?
```

Who is right?


Well, we cannot tell who's wrong or right, can we?

What's strange is that we told Paul and Alice to create a template, and we even told them what data should be displayed (the name of the person).

However, we didn't tell them HOW the data would be passed to their template.

That's the variables.

A template uses some variables.

The data is just as abstract as an idea; it has no body, and no implementation by itself.

To pass the "data" to a template, we define variables.

So, back to our example, if give the following variables:


```php 
$person = [
    "name" => "julie",
    "age" => "30",
];
```

Then, Alice will probably be right.

Okay, now let's talk about the model.

Like with a 3d software, a model is when you digitalizes an object from the real world to your software.

Basically, it's the result of an import.

In our system, it's the same: the model is just a variable that represents an object from the real world.

So, for instance, if we take the $person variable again:

```php 
$person = [
    "name" => "julie",
    "age" => "30",
];
```

we can say that $person is a model representing a person in the real world.

We could have used an oop class if we wanted to, like this:

```php 
$person = new Person();
$person->name = "julie";
$person->age = 30;
```

This model is different from the previous one, but it exposes the same properties.


So, a model can be thought as a convenient way to transport variables from a place to another.

With the power of oop inheritance, it's convenient to create a class, and pass it to the template, via the variables.


Widget and configuration
-----------
2017-02-28


A Widget is used to display a portion of a page.

A Widget is an object which displays a set of variables using a template.

Basically, a Widget api looks like this:

- setVariables (variables)
- setTemplate (templateName)
- render ()


Usually, a widget displays a model, but not always.
 
 
Imagine this more complex template:

```php

Hello {person.name}, how are you?
<?php if(true === $beBold): ?>
    you look very old.
<?php endif; ?>

```

You can see that some variables seem to belong to a person model, but the beBold variable
is different.

There are basically two types of widget variables:

- models
- configuration variables

Models are a great way to pass variables, as they can be re-used easily.

Configuration variables are the other variables: those that don't belong to a model.

So the widget is the glue that holds together the variables (models, or configuration) with a template.


In terms of use, a widget can encompass as little as a string, and as big as a page.
Widgets can be used for instance for:

- a left menu
- a product item in an e-commerce's product page
- an admin page content




Did you notice that if we want to switch the theme of a widget, we just need to change the template?



Layout
------------
2017-02-28

So, in a web page, we have page elements, which are represented by widgets, as we've just seen.

The layout works in a similar way, but at the page structure level.

Basically, the layout is like a template: it's just a string with some placeholders.

That's because we want to be able to change the layout in one click, remember?

However, the placeholders in this case are replaced by widget calls, so a layout looks like this:


```php
<!DOCTYPE html>
<html lang="en">
<head>
    <?php $l->widget('htmlhead'); ?>
</head>

<body>
    <div class="site">
        <div class="top">
            <?php $l->widget('topmenu'); ?>
        </div>
        <div class="body">
            <?php $l->widget('slider'); ?>
            <?php $l->widget('maincontent'); ?>
        </div>
    </div>
</body>
</html>
```

As you can see, in this example I used the variable **$l** to hold the layout.

In terms of code, the Layout should be an object, and widgets should be bound to it.
In this example, my Layout object has a widget method, which role obviously is to display the corresponding widget.

Again, this is because I'm striving for simplicity that the Layout has to be an instance with all the necessary widgets
bound to it from the beginning.

The code has to be decided inside the Controller, and should look like this:

```php
class MyController {

    public function myMethod(){
    
        $layout = new Layout();
        $layout->setTemplate("templateName");
        $layout->bindWidget(HtmlHeadWidget::create());
        $layout->bindWidget(TopMenuWidget::create()->setTemplate("templateA")); 
        $layout->bindWidget(SliderWidget::create()->setVariable("some overriding conf variable", true);
        $layout->bindWidget(MainContentWidget::create());
        
        return Response::createByLayout($layout);
    }

}
```

Basically, the idea is that the Controller can override anything if wants to,
but most of the code is implicitly handled internally by objects such as the Layout object.

For instance in the pseudo code above, we can imagine that the templates of the widgets are set automatically
byt he layout, based on their names, and that there is also a default loader (object which converts the name
of a template into an actual template string), and a default renderer (object that injects the variables into the 
template and returns the rendered result) injected automatically by the Layout.


Also, I assumed that there was an automated variable system that would be able to load the variables (for instance
based on the Widget name, like the loading system of a template).

The implementation remains left to the reader, but the core idea of a Layout is here.

As with the template, notice that to change layout is an easy task: just change the "layout template".

This is only possible because for a given layout, we know which widgets are used.

(That's the same idea of knowing which variables are used for a template.)




Conclusion
------------
2017-02-28

So, this MVC journey was quite long, but worth it.

Now, we know the role of every object.

Remember that a widget can be very big and do a lot of thing, including javascript magic, but it's still just ONE widget.

Don't try do divide it further or you will fall into a MVC hell (trying to answer questions like:
who is supposed to display that javascript call) where things seems to loose their sense: at least
that happened to me.




















  





















