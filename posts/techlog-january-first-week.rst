.. link: 
.. description: 
.. tags: python, Linux, devops
.. date: 2014/01/05 07:32:53
.. title: [techlog] January First Week
.. slug: techlog-january-first-week

Due to popular demand from my humble reader base I am listing the technologies I played around with.

Polished the way I deploy my web apps. Nginx + Supervisord + (uwsgi / gunicorn) + (django/flask) has been my stack for quite some time now. Lately I figured, that having supervisord and uwsgi installed by the operating system's package manager in its own way kind of scatters the app around. Log files can be configured to be where ever you want but almost all the awesome tutorials / blog posts tell you to have the configuration files in /etc itself. Though its not a very big deal to symlink them, why do it when you can have everything around your project directory itself? It also helps in lowering the chances of lack of permission related errors. And my favourite part is that I can put relative paths in all configuration files and be sure that it will work both on my laptop and production ... without much ado.

Came across the awesomeness of python-eve and flask-admin. Eve is a REST api framework for mongodb. I used it for one of my small projects at work to immensely reduce my load. My task was to handled the backend and provide required APIs. If it hadn't been for eve I would have found myself writing a different api for every little request that came my way. But with eve, almost every thing that the consumers of my api wanted could be achieved with little modification to their request parameters ... sorting, filtering, referencing ... name it and it was there.

Thats it for today, stay tuned for more. Adios.