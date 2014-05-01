[Signup for a WebFaction hosting account on the WebFaction website](http://www.webfaction.com?affiliate=kinsa). Once you're signed up login to the control panel with your username and password.

## Setup Databases

In the WebFaction control panel navigate to _Databases_. Create a PostgreSQL database for the project. Make a note of the _database name_, _user_ and _password_ for later.

## Setup the mod_wsgi Application

In the WebFaction control panel navigate to _Domains/Websites_ and then _Applications_. Create a *mod_wsgi* type app for the site using Python 2.7 along with whatever version of mod_wsgi is available for that version of Python.

## Setup Static Media Applications

In the WebFaction control panel navigate to _Domains/Websites_ and then _Applications_. Create a _Static_ type app for static media and a _Static type app for dynamic media. Name the static media app 'static' and the dynamic media app 'media'. Choose an _App type_ of _Static only (no .htaccess)_ and enter _expires max_ into _Extra info_ for both apps.

_If the app isn't named static, a symlink app can serve as a go-between to still use `/static` as the URL for the app. This is kind of weird. An example would be the where the static app is named `awesomesite_static` and has a path on the server of `/home/awesomesiteuser/webapps/awesomesite_static/` and the `awesomesite_static_symlink` app serves as a symlink to `/home/awesomesiteuser/webapps/awesomesite_static/static/` in which case the symlink app gets the url of `/static` when setting up the Website in the WebFaction control panel (as opposed to the actual static app which doesn't get it's own URL)._

## Create the 'Website'

In the WebFaction control panel navigate to _Domains/Websites_ and then _Websites_. Click _Add new website_, give it a name and a domain then from _Add an application_ choose _Reuse an existing application_. Pick your mod_wsgi application from the list and leave the URL empty, click Save. Follow that same method for choosing your static media application, entering a URL of `static` and your dynamic media application, entering a URL of `media`. Save the website.

## Add your SSH key to the WebFaction Server

This makes it so you don't have to continuously input our password when connecting over SSH.

### Copy the key to your WebFaction account

    (vm) $ scp ~/.ssh/id_rsa.pub <accountname>@<accountname>.webfactional.com:temp_id_rsa_key.pub 

When prompted, enter your password.

### Open an SSH session to your account

    (vm) $ ssh <accountname>@<accountname>.webfactional.com 

When prompted, enter your password.

### Create ~/.ssh

    (webfaction) $ mkdir -p $HOME/.ssh

### Add your key to the authorized_keys file

    (webfaction) $ cat ~/temp_id_rsa_key.pub >> ~/.ssh/authorized_keys 

### Remove the temporary key file 

    (webfaction) $ rm ~/temp_id_rsa_key.pub 

### Secure the SSH keys

    (webfaction) $ chmod 600 ~/.ssh/authorized_keys 

### Secure the SSH directory

    (webfaction) $ chmod 700 ~/.ssh 

### Close the SSH session

    (webfaction) $ exit


### Verify that your key works properly. Open an SSH session to your account, you shouldn't be prompted for your password this time

    (vm) $ ssh <accountname>@<accountname>.webfactional.com 

#### Close the SSH session

    (webfaction) $ exit

## Install necessary modules

**Note: use ``python2.7`` to run all commands instead of just `python` or `./`.**

### SSH into your account

    (vm) $ ssh <accountname>@<accountname>.webfactional.com 

Then run:

    (webfaction) $ mkdir -p ~/lib/python2.7
    (webfaction) $ easy_install-2.7 pip 


### Install VirtualEnv and VirtualEnvWrapper

Check whether `virtualenv` and `virtualenvwrapper` are installed, run the following and look for them in the output:

    (webfaction) $ pip freeze

If so, carry on, if not:

    (webfaction) $ pip2.7 install --user virtualenv

If you have to install Virtualenvwrapper, it has to be installed from source ([ref](http://community.webfaction.com/questions/10316/pip-install-virtualenvwrapper-not-working)). The latest version at the time of documentation is 4.1.1… check the list at [https://pypi.python.org/packages/source/v/virtualenvwrapper/]() to verify that before proceeding:

    (webfaction) $ mkdir -p ~/bin ~/lib/python2.7 ~/src
    (webfaction) $ cd ~/src
    (webfaction) $ ln -s $HOME/lib/python2.7 $HOME/lib/python
    (webfaction) $ wget http://pypi.python.org/packages/source/v/virtualenvwrapper/virtualenvwrapper-4.1.1.tar.gz --no-check-certificate
    (webfaction) $ tar zxf virtualenvwrapper-4.1.1.tar.gz
    (webfaction) $ cd virtualenvwrapper-4.1.1
    (webfaction) $ PYTHONPATH=$HOME/lib/python2.7 python2.7 setup.py install --home=$HOME
    (webfaction) $ rm $HOME/lib/python

### Edit the `~/.bashrc` file to reference the virtualenvs 

    (webfaction) $ nano ~/.bashrc

Append at the end:

    export WORKON_HOME=$HOME/.virtualenvs 
    export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python2.7 
    source /home/<accountname>/bin/virtualenvwrapper.sh 
    
If you're on a CentOS 6 machine (your WebFaction server number is >Web300) also add the following ([ref](http://community.webfaction.com/questions/7714/installing-psycopg2-pg_config-missing)):

    export PATH=/usr/pgsql-9.1/bin:$PATH

Exit and save.
 
#### Source the `~/.bashrc` file to load it

    (webfaction) $ source ~/.bashrc


### Configure PIL 

PIP installation of PIL doesn't work so well on WebFaction. (From [http://community.webfaction.com/questions/7340/how-to-install-pil-with-truetype-support]().)

    (webfaction) $ mkdir -p ~/src ~/lib/python2.7
    (webfaction) $ cd ~/src
    (webfaction) $ wget http://effbot.org/media/downloads/PIL-1.1.7.tar.gz
    (webfaction) $ tar zxf PIL-1.1.7.tar.gz
    (webfaction) $ cd PIL-1.1.7

edit PIL's setup.py:

    (webfaction) $ vim setup.py
        
to set the library pointers as follows

    # --------------------------------------------------------------------
    # Library pointers.
    #
    # Use None to look for the libraries in well-known library locations.
    # Use a string to specify a single directory, for both the library and
    # the include files.  Use a tuple to specify separate directories:
    # (libpath, includepath).  Examples:
    #
    # JPEG_ROOT = "/home/libraries/jpeg-6b"
    # TIFF_ROOT = "/opt/tiff/lib", "/opt/tiff/include"
    #
    # If you have "lib" and "include" directories under a common parent,
    # you can use the "libinclude" helper:
    #
    # TIFF_ROOT = libinclude("/opt/tiff")

    TCL_ROOT = None
    JPEG_ROOT = '/usr/lib64','/usr/include'
    ZLIB_ROOT = '/lib64','/usr/include'
    TIFF_ROOT = None
    FREETYPE_ROOT = '/usr/lib64','/usr/include/freetype2/freetype'
    LCMS_ROOT = None

build PIL with the following command:

    (webfaction) $ python2.7 setup.py build_ext -i
    
run the test to confirm that the build was successful:

    (webfaction) $ python2.7 selftest.py
    
should return:

    --------------------------------------------------------------------
    PIL 1.1.7 TEST SUMMARY 
    --------------------------------------------------------------------
    Python modules loaded from ./PIL
    Binary modules loaded from ./PIL
    --------------------------------------------------------------------
    --- PIL CORE support ok
    --- TKINTER support ok
    --- JPEG support ok
    --- ZLIB (PNG/ZIP) support ok
    --- FREETYPE2 support ok
    --- LITTLECMS support ok
    --------------------------------------------------------------------

install the library:

    (webfaction) $ python2.7 setup.py install

### Create the virtualenv

    (webfaction) $ mkdir -p ~/.virtualenvs/ 
    (webfaction) $ mkvirtualenv <envname>
    (webfaction) $ workon <envname> && cdvirtualenv
    (webfaction) $ add2virtualenv .

### Correct the permissions for the media app

    (webfaction) $ chmod 755 ~/webapps/<dynamic_media_application_name>

### Edit the .wsgi file

Create a wsgi file in the mod_wsgi application:

    $ nano ~/webapps/<wsgi_application_name>/<wsgi_application_name>.wsgi

Edit it to include:

    #!/usr/bin/python 
    import os
    import site
    import sys  

    # Tell wsgi to add the Python site-packages to it's path. 
    site.addsitedir('/home/<accountname>/.virtualenvs/<envname>/lib/python2.7/site-packages')  

    # Fix markdown.py (and potentially others) using stdout 
    sys.stdout = sys.stderr  

    # Append the Django project to the path. 
    sys.path.append('/home/<accountname>/.virtualenvs/<envname>/<django_project_name>')  

    # On Django 1.4 projects settings now lives in an application (generally named myproject) within the project (also named myproject) so this should be 'myproject.settings.production', on older Django installations (Django 1.3 say) this should just be 'settings'
    os.environ['DJANGO_SETTINGS_MODULE'] = 'myproject.settings.production' 
    from django.core.handlers.wsgi import WSGIHandler 
    application = WSGIHandler()  

### Edit the apache config file

    $ nano ~/webapps/<wsgi_application_name>/apache2/conf/httpd.conf

Comment out the `DirectoryIndex` and `DocumentRoot` lines.

At the bottom of the file, add the following where port number is the port number assigned for the mod_wsgi application (which can be found by selecting the mod\_wsgi application from the list of Applications in the WebFaction Control Panel under Domains/Websites):

    NameVirtualHost *:<port_number>
    <VirtualHost *:<port_number>>
        ServerName <domain_name.com>
        ServerAlias www.<domain_name.com> <alternate_domain_name.com>
        WSGIScriptAlias / /home/<accountname>/webapps/<wsgi_application_name>/<wsgi_application_name>.wsgi
    </VirtualHost>

#### (Optional) Configure Domain Aliases

Make sure that the Apache rewrite_module is being loaded, there should be a line at the top that looks like:

    LoadModule rewrite_module modules/mod_rewrite.so

In the `VirtualHost`, before the closing tag (`</VirtualHost>`), add the following:

Turn on the RewriteEngine:

    RewriteEngine On

Send access attempts to 'www' to the domain without that subdomain (make sure that 'www.domainname.tld' is setup under the domains tab of the WebFaction Domains control panel and is pointed at the app). Replace `domainname.tld` with the real domain name and top level domain:

    RewriteCond %{HTTP_HOST} ^www.domainname.tld$ [NC]
    RewriteRule ^(.*)$ http://domainname.tld$1 [R=301,L]

Point an alternate domain at our domain (make sure that 'www.otherdomainname.tld' and 'otherdomainname.tld' are setup under the domains tab of the WebFaction Domains control panel and are pointed at the app). Replace `otherdomainname.tld` with the other domain name and top level domain and replace `domainname.tld` with the primary domain name and top level domain:

    RewriteCond %{HTTP_HOST} ^otherdomainname.tld$ [NC]
    RewriteRule ^(.*)$ http://domainname.tld$1 [R=301,L]

    RewriteCond %{HTTP_HOST} ^www.otherdomainname.tld$ [NC]
    RewriteRule ^(.*)$ http://domainname.tld$1 [R=301,L]

#### Reboot Apache

Verify that works by rebooting (site won't load yet, need to do a deploy, but restart should go without errors).

    (webfaction) $ ~/webapps/<wsgi_application_name>/apache2/bin/restart

### (Optional) .htpasswd protect site

From [http://httpd.apache.org/docs/2.0/howto/auth.html]() AND [http://community.webfaction.com/questions/256/apache-basic-authentication-for-mod_wsgi-inc-django-applications]():

Make a directory to store the .htapasswd files:

    (webfaction) $ mkdir -p /home/<username>/webapps/<webapp name>/apache2/passwd

Create the .htpasswd file (which in this case is a file called 'passwords' using the htpasswd command:

    (webfaction) $ htpasswd -c /home/<username>/webapps/<webapp name>/apache2/passwd/passwords <htpasswd user's username>

Edit the httpd.conf file to include the necessary lines:

    (webfaction) $ nano /home/<username>/webapps/<webapp name>/apache2/conf/httpd.conf

And append:

    LoadModule auth_basic_module modules/mod_auth_basic.so
    LoadModule authn_file_module modules/mod_authn_file.so
    LoadModule authz_user_module modules/mod_authz_user.so
    <Location />
        AuthType Basic
        AuthName "Authentication Required"
        AuthUserFile /home/<username>/webapps/<webapp name>/apache2/passwd/passwords
        Require valid-user
    </Location>

Restart the server:

    $ /home/<username>/webapps/<webapp name>/apache2/bin/restart

### Schedule a Regular Database Backup

From [http://docs.webfaction.com/user-guide/databases.html]().

    (webfaction) $ nano ~/.pgpass

Add a new line containing the following, where `database_name` is the name of the database as it appears on the control panel, `database_user` is the user you created for the database and `password` is the database password:

    *:*:database_name:database_user:password

Secure the ~/.pgpass file. 

    (webfaction) $ chmod 600 ~/.pgpass

Create a directory to store the database backups. 

    (webfaction) $ mkdir -p ~/db_backups

Edit your crontab:

    (webfaction) $ crontab -e

To include, replacing `databaseUser` with the user you created for the database and `databaseName` with the name of the database (occurs multiple times). Adjust the frequency as desired. This will backup the database once a day at 0800 UTC (or whatever the system clock is set to).:

    0 8 * * * /usr/local/pgsql/bin/pg_dump -Ft -b -U databaseUser databaseName | gzip -9 > $HOME/db_backups/databaseName-`date +\%Y\%m\%d`.sql.gz 2>> $HOME/db_backups/backups.log && echo "Database backup completed successfully on `date`" >> $HOME/db_backups/backups.log

**NOTE:** If the above line executes when run directly in the Bash shell but not when executed via crontab, the leading cause is the escaping of the timestamp. So, if it is not working, try unescaping the timestamp by removing the backslashes.

### Configure memcached

Based on: [http://docs.webfaction.com/software/memcached.html](), [http://docs.webfaction.com/software/django/config.html#django-memcached](), [http://forum.webfaction.com/viewtopic.php?pid=2311](), and [http://docs.webfaction.com/software/python.html#installing-packages-with-setup-py]().

_A supposedly much faster alternative to python-memcached is pylibmc. I have yet to figure out how to install it though._

Make sure not to be in a virtual environment:

    (webfaction) $ deactivate

Create `~/src` if it doesn't exist yet:

    (webfaction) $ mkdir -p ~/src

Install and configure:

    (webfaction) $ cd ~/src
    (webfaction) $ wget ftp://ftp.tummy.com/pub/python-memcached/python-memcached-latest.tar.gz
    (webfaction) $ tar -xzvf python-memcached-latest.tar.gz

`cd` into the directory that was created when you unpacked: 

    (webfaction) $ cd python-memcached-1.53
    (webfaction) $ python2.7 setup.py build
    (webfaction) $ python2.7 setup.py install --home=$HOME

Enable memcached as a daemon (defaults to using 64mb of memory; run `memcached -h` for optional configuration parameters including the amount of memory to use)

    (webfaction) $ memcached -d -s ~/memcached.sock

Establish that memcached is running (where `username` is your WebFaction account username):

    (webfaction) $ ps -u <username> -o pid,command | grep memcached

### Close the SSH session

    (webfaction) $ exit

## Edit the project's environmental settings file

In `settings/<environment name>.py`, update the following configuration variables, replacing <accountname> with your WebFaction account name and <url> with your website's URL (including any variants such as _accountname.webfaction.com_):

    MEDIA_ROOT = '/home/<accountname>/webapps/<dynamic_media_app_name>/'
    MEDIA_URL = '/media/'

    STATIC_ROOT = '/home/<accountname>/webapps/<static_media_app_name>/'
    STATIC_URL = '/static/'

    ADMIN_MEDIA_PREFIX = '/static/admin/'
    
    ALLOWED_HOSTS = [<url>]

    CACHES = {
            'default': {
                'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
                'LOCATION': 'unix://unix:/home/<username>/memcached.sock',
            }
    }

## Edit the project's fab file

In `fabfile.py`, in the environment you are configuring (probably `production` but possibly `staging` or something else) replace `<accountname>` with your WebFaction account name, `<envname>` with your virtualenv name, `<wsgi_application_name>` with the name of the WSGI application you created and adjust the name of the `env.remote_static_root` as necessary based on the name of the static app created. In the `production()` method update the following variables:

    env.apache_restart_command = '/home/<accountname>/webapps/<wsgi_application_name>/apache2/bin/restart'
    env.hosts = ['<accountname>@<accountname>.webfactional.com']
    env.password = '<password>'
    env.virtualenv_name = '<envname>'
    env.path = '/home/<accountname>/.virtualenvs/<virtualenv_name>'
    env.remote_media_root = '/home/<accountname>/webapps/media'
    env.remote_static_root = '/home/<accountname>/webapps'

## Setup the environment on the server and do an initial deployment

Replace `<environemnt name>` with the name of the environment as configured in `fabfile.py`, probably `production`.

    (vm) $ fab <environment name> remote_setup
    
### Freeze production requirements

    (vm) $ sudo pip freeze > requirements/<environment name>.txt
    (vm) $ git add requirements/<environment name>.txt
    (vm) $ git commit requirements/<environment name>.txt -m "adding <environment name> requirements"

### Deploy

    (vm) $ fab <environment name> deploy

_Watch the output the first time debugging. If PIL installs without libjpeg support, follow these directions: [http://community.webfaction.com/questions/7340/how-to-install-pil-with-truetype-support]()._
