Kam
=================
2017-02-27 -> 2018-06-27




In 2017-02-27, I started a new framework named [kam](https://github.com/lingtalfi/kam/blob/master/README-2018-06-27.md).
After implementing it (in the form of framework named kamille),
I realized that some of the kam concepts have not been used, or used differently than what was planned at the beginning.


Today, I want to re-expose the idea of the kam framework.
It's nothing but an update of the kam concepts told by a more experimented soul, an upgraded version of the original kam idea if you will.



So what I really meant with kam was this...
------------------



kam is a mouthful idea: it's the idea that an application handles requests.
It can be a web application or a console application, but still, it will only handle requests.

The application provides entry points to let the external world interact with it (which is the whole point of an app).

The handling of a request is quite peculiar too.

When an application receives a request, it analyses it, and dispatches it to a controller,
which is responsible for providing an appropriate response for a very specific request.

For web apps, the request is most likely an http request, and the analysis of the request is done via a router, which
is the name of a mechanism for finding the controller matching a certain http request.

The controller might return the appropriated response to the application, which is then transmitted to the request issuer.

In case of a controller failure, or any other problem, the application is responsible for informing the request issuer
than something wrong occurred.

In order to do its things, the application will use modules.
A module is some code that brings new capabilities to the application.

A module can be installed/uninstalled, so that we (as developers) can augment/diminish the capabilities of
the application without changing its core fundamentally.



So, all this, this is the kam idea.

And so from now on we can refer to this idea as such.



The kamille planet
----------------------

In order to help creating such an application, I've created a planet called [kamille](https://github.com/lingtalfi/kamille).

Kamille is a collection of tools that help implement the kam framework at the library level.



The kamille application
--------------------------

I'm also currently working on the concrete implementation of a kamille application that one could copy paste and go.
It's [here](https://github.com/lingtalfi/kamille-app).




