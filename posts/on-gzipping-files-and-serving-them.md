.. link: 
.. description: 
.. tags: nginx,
.. date: 2014/04/14 07:32:53
.. title: On gzipping files and serving them.
.. slug: on-gzipping-files-and-serving-them

Even though configuring servers is an integral part of my life as a web dev, there are some nuances which elude me every now and then. Recently I tried to serve some gzipped files via nginx and hit a road block. The problem statement was simple, I had some files that I had compressed using gzip and saved on the disk of my server, coz my files took 80% less space on the disk compressed than when they were not. And because python makes gzipping so simple I am practically compressing every dump-file and saving on space on the server and time transferring it. Here is how, 

    import gzip
    my_data = "<insert your favourite content>"
    with gzip.open( "my_awesome_file.txt.gz", 'w') as f:
        f.write(my_data)

Similarly the file can be opened in 'r' mode if the aim is to read it. Sorry, I got carried away, coming back to the point of serving them via nginx.
I had a problem, so I asked uncle google. However, every result that he came up with was regarding compressing and then serving static files and in my case the files were already compressed. After some tinkering I did figure out my mistake. I was setting up the content-type as 'gzip', which the browser was not able to display and hence was prompting to download as binary file. Where as what I had to do was specify content-encoding as gzip. Here is the snippet that helped me get it done:


    location /cache/ {
            alias /path/to/the/directory/cache/;
            expires     max;
            add_header  Content-Type text/html;
            add_header  Content-Encoding gzip;
            add_header  Cache-Control public;
            add_header  Last-Modified "";
            add_header  ETag "";
    }

I have a couple of small instances on Digital Ocean where I run my twisted based crawlers. And more often than not, there is a need to look at the page that was downloaded when it was downloaded, in order to determine whether there is an error in the parser or the page actually did not have the information that was supposed to be extracted. Since these instances had virtually nothing except Twisted on them, I was reluctant to install apache / nginx just to occasionally serve some cached html pages. Initially, I wasn't compressing them and they ended up consuming the entire disk space of 20 gigs. I found that twisted's built in web server was enough to serve the files:

    twistd web --path /path/to/web/content

Soon the disk space proved to be very insufficient, flushing the files became a pain. Though a simple cron job or a fabric script would have rid me from the drudgery of running the rm command on each of those crawlers, I didn't quite feel like doing that. Looking for an alternative, I found gzip ... which allowed me to compress the htmls to almost 90% in some cases and 70-80 percent on an average using the script I illustrated above. Then there was a new problem, tiwsted web couldn't serve the gzipped files right out of the box. This meant I had a problem ... so ... yes you guessed it right, I went to uncle google. After shuffling through the results I was able to stitch up this small script that serves the gzipped html files just fine and with the proper content encoding they are also automatically uncompressed by the browser after being downloaded. Without much ado, I present to you the script:


    # to be saved with an .rpy extension
    # eg serve_something.rpy
    from twisted.web.resource import Resource

    class MyResource(Resource):
        is_leaf=True
        def render_GET(self, request):
            request.setHeader("content-type", "text/html")
            request.setHeader("content-encoding", "gzip")
            print request.args
            filename = request.args["id"][0]
            return open("/path/to/directory/contaning/cached/%s"%filename).read()

    resource = MyResource()

This needs to be run using following command :

    twistd --pidfile=tweb.pid web --path=/path/to/the/directory/having/the/rpy/file --port=8888

And it works like a charm, making me one happy python developer. :)
