
-------------------------------------------------------------------------------------------------------------------------------------
Optional. Background Processing with django-celery utilizing redis as a broker and supervisord as a daemon. 

Create two new custom apps of type "listening on port." The first will be for redis and the second will be for supervisord. Record the port numbers assigned to both of these. On the supervisord app setup a website for that application as well (probably at a subdomain) so you can view the web interface for supervisor.

Add the following to the requirements list and install:
celery
celery-with-redis
django-supervisor

Add the following to the list of installed apps in the project's settings file. The first line is required for Celery, the second to use the Django database as the broker:
'djcelery',
'djsupervisor',

In the project's settings file add the following. We're telling it to use the 2nd redis database here because this example is taken from a config where the redis database 0 is used for something else, change to /0 for the first redis database or pick an available database number.:
# django celery
djcelery.setup_loader()
TEST_RUNNER = 'djcelery.contrib.test_runner.CeleryTestSuiteRunner'
CELERYD_CONCURRENCY = 1
CELERYD_NODES = "w1"

BROKER_URL = 'redis://localhost:<redis custom app port number>/1'

# django supervisord
SUPERVISORD_PORT = <supervisord custom app port number (as integer, not string)>

The reason we set the concurrency so low is because Celery takes up a good amount of memory, and you are likely limited with your memory consumption on WebFaction. The minimum amount of memory Celery can take will be however much it needs to run the main process (consuming messages, sending tasks to workers, etc), and a worker tasks that actually does stuff. Each of these will take up about 20-30MB of memory depending on the size of your Django app.

Create the tasks (file(s)) (http://docs.celeryproject.org/en/latest/django/first-steps-with-django.html#defining-and-calling-tasks).

In the application's mod_wsgi file add (the djcelery part might not be required - this already happens in settings, not sure where it is supposed to go):
import djcelery
import os
os.environ["CELERY_LOADER"] = "django"
djcelery.setup_loader()

Add the following to the .gitignore file:
supervisord.log
supervisord.pid

Create a supervisord.conf script in the project directory next to manage.py:
$ vim supervisord.conf

Add to that (note the first if/else statement is specific here to the MercuryCSC site and is used to set a staging/production switch, this requires staging and production to be on separate webfaction machines. Also note that the username and password are just used to setup a .htpasswd file governing access to the supervisord web admin. (This is pulled heavily from the django-supervisord documentation and this post: http://fightingrabbits.com/archives/208):
[program:celeryd]
{% if environ.HOSTNAME == 'web368.webfaction.com' %}
command={{ PYTHON }} {{ PROJECT_DIR }}/manage.py celeryd --settings=myproject.settings.staging -l info ; this works
{% else %}
command={{ PYTHON }} {{ PROJECT_DIR }}/manage.py celeryd --settings=myproject.settings.production -l info ; this works
{% endif %}

; Disable auto-reloading on code changes by excluding that program.
[program:autoreload]
exclude=true

[program:runserver]
exclude=true

[program:celerybeat]
exclude=true

[inet_http_server]                          ; inet (TCP) server setings
port=127.0.0.1:{{ settings.SUPERVISORD_PORT }}                 ; (ip_address:port specifier, *:port for all iface)
username=mercurycsc                         ; (default is no username (open server))
password=22sgrandave                            ; (default is no password (open server))
 
[supervisord]
logfile=/home/rottenxmas/logs/user/supervisord.log         ; (main log file;default $CWD/supervisord.log)
logfile_maxbytes=50MB                               ; (max main logfile bytes b4 rotation;default 50MB)
logfile_backups=10                                  ; (num of main logfile rotation backups;default 10)
loglevel=debug                                      ; (log level;default info; others: debug,warn,trace)
pidfile=/home/rottenxmas/supervisord.pid                                  ; (supervisord pidfile;default supervisord.pid)
nodaemon=false                                      ; (start in foreground if true;default false)
minfds=1024                                         ; (min. avail startup file descriptors;default 1024)
minprocs=200                                        ; (min. avail process descriptors;default 200)
 
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
 
[supervisorctl]
serverurl=http://127.0.0.1:{{ settings.SUPERVISORD_PORT }}
username=mercurycsc
password=22sgrandave

Update the fabfile. Add the following private method:
def _restart_supervisord():
    "Restart Supervisord"
    run('cd %(path)s/%(project)s; workon %(virtualenv)s; '
        'python2.7 manage.py supervisor --settings=%(settings_path)s --daemonize' % {
            'path': env.path, 'project': env.project_name,
            'virtualenv': env.virtualenv_name,
            'settings_path': env.settings_path})

Add the following public method:
def restart_supervisord():
    _reload_apache()
    _restart_supervisord()

Update the _deploy() method to include _restart_supervisord() after the _reload_apache() method.

Update the cron script to execute those automatically:
19,39,59 * * * * $HOME/.virtualenvs/merc_staging/bin/python /home/rottenxmas/.virtualenvs/merc_staging/myproject/manage.py supervisor --settings=myproject.settings.staging --daemonize

Update the project's Procfile.dev to run redis and celery in development. Add:
redis: redis-server
celery: python manage.py celery worker --loglevel=info
