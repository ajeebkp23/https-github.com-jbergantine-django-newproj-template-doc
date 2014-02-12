remove _xapian_ line from the bootstrap.sh file

remove _xapian_ line from the Vagrantfile file

remove _memcached_ line from the bootstrap.sh file

remove _memcached_ line from the Vagrantfile file

replace the _chef-cookbook-djangonewproj_ line in vagrantfile with:

    chef.add_recipe "chef-cookbook-djangonewproj::virtualenv"
    chef.add_recipe "chef-cookbook-djangonewproj::bash"
    chef.add_recipe "chef-cookbook-djangonewproj::symlink-pil"
    chef.add_recipe "chef-cookbook-djangonewproj::postgresql"
    chef.add_recipe "chef-cookbook-djangonewproj::rubygems"

`$ vagrant up`

`$ subl .`

`$ vagrant ssh`

create a `requirements.txt` file (from [http://docs.django-cms.org/en/2.4.3/getting_started/installation.html]())

`$ touch requirements.txt`

	django-cms==2.4.1

	#These dependencies are brought in by django-cms, but if you want to lock-in their version, specify them
	Django==1.5.1
	django-classy-tags==0.4
	South==0.8.1
	html5lib==1.0b1
	django-mptt==0.5.2
	django-sekizai==0.7
	six==1.3.0

	#Optional, recommended packages
	Pillow==2.0.0
	django-filer==0.9.4
	cmsplugin-filer==0.9.5
	django-reversion==1.7

	psycopg2==2.5

install that

`$ sudo pip install -r requirements.txt`

From here, follow the [directions](http://docs.django-cms.org/en/2.4.3/getting_started/tutorial.html). The postgres database is `django_db`, the user is `django_login` and the password is `secret`.