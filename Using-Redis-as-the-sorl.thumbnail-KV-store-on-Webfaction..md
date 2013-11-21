Install redis locally using the cookbook instructions with django-newproj.

Install the redis python project using pip

    $ sudo pip install redis

Make sure to add that to the requirements file.

From the WebFaction control panel create a new custom app listening on port and copy down the port number. We'll use it in a second.

SSH in and move to `~/src` (making it if necessary)

    $ mkdir -p ~/src && cd $_

Install (from [http://redis.io/download]())

    $ wget http://redis.googlecode.com/files/redis-2.6.13.tar.gz
    $ tar xzf redis-2.6.13.tar.gz
    $ cd redis-2.6.13
    $ make

Edit the .conf file (at `~/src/redis-2.6.13/redis.conf`)

    $ vim redis.conf

Change `daemonize no` to `daemonize yes`.
Change `port 6379` to `port <custom application port number>`

Save and quit.

Copy redis.conf, redis-server and redis-cli to ~/bin

    $ cp redis.conf ~/bin/
    $ cd src && cp redis-server ~/bin/
    $ cp redis-cli ~/bin/

Start the server

    $ cd ~/bin
    $ ./redis-server redis.conf

Test it out

    $ ./redis-cli -p <custom application port number> ping

It should respond    

    > PONG

Then in the Django project's settings file, tell sorl.thumbnail to use Redis and listen at the right port

    # sorl thumbnail
    # http://sorl-thumbnail.readthedocs.org/en/latest/reference/settings.html#thumbnail-kvstore
    THUMBNAIL_KVSTORE = 'sorl.thumbnail.kvstores.redis_kvstore.KVStore'
    THUMBNAIL_REDIS_PORT = <custom application port number>

Finally, in the crontab, set a process to make sure it is running

    $ crontab -e

And add at the bottom

    17,37,57 * * * * ~/bin/redis-server ~/bin/redis.conf
