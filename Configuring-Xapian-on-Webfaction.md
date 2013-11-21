Make sure not to be in a virtual environment:

    $ deactivate

Create ~/src if it doesn't exist yet:

    $ mkdir ~/src

Install and configure:

    $ cd ~/src
    $ wget http://oligarchy.co.uk/xapian/1.2.6/xapian-core-1.2.6.tar.gz
    $ wget http://oligarchy.co.uk/xapian/1.2.6/xapian-bindings-1.2.6.tar.gz
    $ tar -xzvf xapian-core-1.2.6.tar.gz
    $ tar -xzvf xapian-bindings-1.2.6.tar.gz

    $ cd xapian-core-1.2.6
    $ mkdir -p $HOME/bin/xapian
    $ ./configure --prefix=$HOME/bin/xapian
    $ make
    $ make install

    $ cd ../xapian-bindings-1.2.6
    $ ./configure --prefix=$HOME/bin/xapian-bindings XAPIAN_CONFIG=$HOME/bin/xapian/bin/xapian-config  PYTHON=/usr/local/bin/python2.7 PYTHON_LIB=$HOME/lib/python2.7 --with-python --without-ruby --without-php --without-tcl 
    $ make
    $ make install

Test that:

    $ python2.7 -c "import xapian"

Configure the Django project to point at the correct spot:
in settings.py point `HAYSTACK_XAPIAN_PATH` at `'/home/<username>/bin/xapian'`

Setup a crontab to rebuild and update the index:

    # rebuild search index once a day
    3 3 * * * (echo `date` && /home/<webfaction_username>/.virtualenvs/<virtualenv_name>/bin/python2.7 /home/<webfaction_username>/.virtualenvs/<virtualenv_name>/myproject/manage.py rebuild_index --settings=myproject.settings --noinput) 2>&1 >> /home/<webfaction_username>/mycron.log

    # update search index every 5 minutes except during the 3 o'clock hour
    */5 * * * * (echo `date` && /home/<webfaction_username>/.virtualenvs/<virtualenv_name>/bin/python2.7 /home/<webfaction_username>/.virtualenvs/<virtualenv_name>/myproject/manage.py update_index --settings=myproject.settings) 2>&1 >> /home/<webfaction_username>/mycron.log
