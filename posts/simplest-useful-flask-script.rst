.. link: 
.. description: 
.. tags: python, requests, flask
.. date: 2013/12/24 02:09:27
.. title: Simplest useful flask script
.. slug: simplest-useful-flask-script

Usually django is my first choice when the task at hand is a web project. I have also used django for some projects which weren't actually meant to be web applications and been benefited by the plethora of utilities that come along with it. However, every once in a while you come across simple one time tasks for which django feels like a huge unnecessary monster bloatware. Something similar happened today. 

So there is this improperly documented third-party API that was to be integrated into one of the applications of our company. And this API provides a callback feature, such that when you send it an asynchronous request, it would after some time, post some data back to a URL you provide. Hovever, the format and structure of the data that would would be posted is not documented and the point of contact of the API provider is also not sure about it. 

Other endpoints of the api which required just a GET or a POST request were easily explored and integrated with the help of Postman_. (the awesome chrome plugin and python-requests). But this callback needed a url where it would post data and I didn't quite feel like tampering the code of any of the existing hosted applications. Hence came this small and elegant python script, which does nothing more that printing whatever was posted to it.

.. gist:: 8104480

In django, its quite a cumbersome task, if all you want to do is this. So here I am, sharing a silly little piece of code that simplified my life today.


.. _Postman: https://chrome.google.com/webstore/detail/postman-rest-client/