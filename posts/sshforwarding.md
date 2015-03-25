<!-- 
.. link: 
.. description: How to use ssh forwarding to simplify deployment process.
.. tags: Linux, deployment, devops, github, bitbucket.
.. date: 2015/01/18 19:06:08
.. title: SSH forwarding vs deployment keys
.. slug: ssh-forarding-vs-deployment-keys
-->


There comes a time in the life of every web application when it has to get deployed and be made accessible for the masses. Deployment, among other steps like setting up the database and configuring the web server, comprises primarily of getting the source code to the server. Many prefer creating an archive of the source code on the local system and copying it over to the remote machine. Though this is the simplest method, its horrors start to show up with the increase in the size of the code base and the number of developers in the team. 

There comes a time, when keeping the people taking care of deployment need to be sure that the source code on the server is also the freshest one from the master branch of the source version control system. Services like github and bitbucket provide a utility called deployment keys - What one needs to do is generate a public-private key pair and add the public key to the repository as a 'deployment key' for the project. This makes it possible to clone / pull the source directly from the repository. But when there are more than one server that the source code needs to be deployed to 
    - well either a public key from each of those servers is added as deployment key.
    - or the private key is copied to the ~/.ssh/ directory on all the servers.


There is a way to avoid all that work - by enabling SSH Forwarding. The concept is pretty simple. When a user logs in to the server remotely via SSH, the session on the remote server is able to access the private keys of the user's workstation for accessing other machines through this one. This is also the basic concept behing SSH tunnelling.


All one needs to do is modify ~/.ssh/config file and add something like:

    Host anuvrat.in
        ForwardAgent yes

or 

    Host ip.add.re.ss
        ForwardAgent yes

or 

    Host *.yourcompany.com
        ForwardAgent yes


The benefit is that all the remote machines that you ssh into, provided they match the host parameter can now authorize themselves against github / bitbucket and get the source code. 


references:

-   <https://developer.github.com/guides/using-ssh-agent-forwarding/>
-   <https://developer.github.com/guides/managing-deploy-keys/>
-   <https://confluence.atlassian.com/display/BITBUCKET/Use+deployment+keys>







