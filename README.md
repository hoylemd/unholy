unholy
------

This is an ansible playbook that will configure all of your media management needs on an Ubuntu linux server (tested with 13.10).

It installs nginx, sabnzbd, sickbeard, couchpotato, transmission and plex media server.

Usage
-----

http://asciinema.org/a/8196

If you're familiar with ansible, all you need to do is edit the vars.yml file,
check all of the roles to see if they're configured to your liking, make an inventory file and run `ansible-playbook -i inventory main.yml`.

If you've never used ansible, here are some basic instructions:

    # install ansible
    sudo pip install ansible  # people tend to bitch about pip. you can use apt or brew too.
    # add your server to an inventory file
    nano unholy
    ansible-playbook -i unholy --ask-sudo-pass -u your_user main.yml

Testing
-------

If you modify something, you can test that everything works with vagrant.
Just run `vagrant up` and then `vagrant provision` when you need to re-run ansible.
The Vagrantfile forwards port 443 to 8080 on your machine; it also forwards 8000,
8010,8020, 8030, and 32400.

Nginx
-----

Nginx is configured to use https and redirect all http requests to https.
Afaik, https is configured using the current best security practices,
that is, TLS 1.2 and 1.1 only with AES-GCM used as the most preferable cipher.

It will generate a self-signed SSL certificate for you and put it in /etc/nginx as
server.crt and server.key. I haven't added any way to use your own cert, mainly
because I'm too cheap to buy a cert from a CA.

Sabnzbd
-------

You'll find sabnzbd at https://your_server/sabnzbd
Sabnzbd doesn't allow for configuration of a url-base/web-root, so on initial setup,
it won't redirect to the wizard properly. To fix this, just manually append the wizard path. In other words, go here: https://your_server/sabnzbd/wizard.

Sickbeard
---------

Sickbeard should work just fine out of the box, go to https://your_server/sickbeard.

Couchpotato
-----------

Couchpotato should also *just work*, find it at https://your_server/couchpotato

Transmission
------------

Transmission is at https://your_server/transmission

Plex
----

Plex currently isn't proxied behind nginx, since you can access your server just fine from http://plex.tv after it's been configured.
To configure, go to http://your_server:32400/web and give it your plex account info.
