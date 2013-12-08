## Install MySQL Server and create a user and database for Django to interact with

    $ sudo apt-get install mysql-server
    $ mysql --user=root mysql -p
    mysql> CREATE USER 'django_login' IDENTIFIED BY 'secret';
    mysql> CREATE DATABASE django_db CHARSET UTF8;
    mysql> GRANT ALL PRIVILEGES ON django_db.* TO "django_login"@"localhost" IDENTIFIED BY "secret";
    mysql> \q

## MySQLdb can't be installed via PIP directly, use the Ubuntu apt package instead and then install 

    $ sudo apt-get build-dep python-mysqldb
    $ sudo pip install MySQL-python