# Overview

These directions are an alternative to those provided in the Readme for moving an existing project into Vagrant while using the project's cookbooks and Vagrantfile but not the new project template.

These directions start a new Git project which may not always be ideal.

These same instructions could be used to combine the Vagrantfile and cookbooks to provision a development environment for a non Django Python project or a Django project that needs to follow other conventions than those provided in the project template (say, for a deployment to Heroku). They could also be used as the base for provisioning a development environment for some entirely new technology (say, Ruby on Rails).

# Directions

1. Create a directory for the project and `cd` into it
2. Initialize a Git project: `(local) $ git init && mkdir cookbooks`
3. Load cookbooks: `(local) $ git submodule add git://github.com/opscode-cookbooks/apt.git cookbooks/apt; git submodule add git://github.com/opscode-cookbooks/build-essential.git cookbooks/build-essential; git submodule add git://github.com/opscode-cookbooks/sudo.git cookbooks/sudo; git submodule add git://github.com/opscode-cookbooks/git.git cookbooks/git; git submodule add git://github.com/opscode-cookbooks/openssl.git cookbooks/openssl; git submodule add git://github.com/opscode-cookbooks/postgresql.git cookbooks/postgresql; git submodule add git://github.com/comandrei/python.git cookbooks/python; git submodule add git://github.com/opscode-cookbooks/zlib.git cookbooks/zlib; git submodule add git://github.com/opscode-cookbooks/memcached.git cookbooks/memcached; git submodule add git://github.com/jbergantine/chef-cookbook-python-psycopg2.git cookbooks/chef-cookbook-python-psycopg2; git submodule add git://github.com/jbergantine/chef-cookbook-libjpeg.git cookbooks/chef-cookbook-libjpeg; git submodule add git://github.com/jbergantine/chef-cookbook-libfreetype.git cookbooks/chef-cookbook-libfreetype; git submodule add git://github.com/jbergantine/chef-cookbook-xapian.git cookbooks/chef-cookbook-xapian; git submodule add git://github.com/jbergantine/chef-cookbook-djangonewproj.git cookbooks/chef-cookbook-djangonewproj`
4. Git submodule init and update: `(local) $ git submodule init; git submodule update`
5. Get the Vagrant file: `(local) $ curl https://gist.github.com/jbergantine/3875868/raw/gistfile1.rb > Vagrantfile`
6. Open that (substitute `subl` for command to open in text editor of choice): `(local) $ subl Vagrantfile` 
7. Modify line 79, `chef.add_recipe "chef-cookbook-djangonewproj"` to be:
    
    ```
    chef.add_recipe "chef-cookbook-djangonewproj::virtualenv"
    chef.add_recipe "chef-cookbook-djangonewproj::bash"
    chef.add_recipe "chef-cookbook-djangonewproj::symlink-xapian"
    chef.add_recipe "chef-cookbook-djangonewproj::symlink-pil"
    chef.add_recipe "chef-cookbook-djangonewproj::postgresql"
    chef.add_recipe "chef-cookbook-djangonewproj::rubygems"
    chef.add_recipe "chef-cookbook-djangonewproj::init-git"
    ```

8. Provision Vagrant: `(local) $ vagrant up`
9. **Locally:** download the repo and move it to `myproject` within the project directory:

    a. Open a finder window in your current working directory `open .`

    b. Download the project zip file from GitHub or BitBucket

    c. Copy and paste the project from Downloads into the working directory opened in step 1a. and rename it `myproject`

    d. Prepare for editing `subl .`

10. SSH into Vagrant: `(local) $ vagrant ssh`
11. Copy over SSH keys and secure:

    a. `(ve) $ vi ~/.ssh/id_rsa.pub`

    b. Open a new Terminal tab and copy your local public key: `(local) $ pbcopy ~/.ssh/id_rsa.pub`

    c. Switch to the Terminal tab open to the SSH session of the VE and paste. Save and quit.

    d. Open the private key for editing in the VE: `(ve) $ vi ~/.ssh/id_rsa`

    e. Moving back to the local Terminal tab, copy the private key: `(local) $ pbcopy ~/.ssh/id_rsa`

    f. Switch to the Terminal tab open to the SSH session of the VE and paste. Save and quit.

    g. Protect the private key: `(ve) $ chmod 600 ~/.ssh/id_rsa`

12. In Sublime (or whatever text editor you opened in Step 9d configure the project to work with the virtual environment. Specifically, things that are likely to be out of whack are the development database settings, how settings are setup (whether they're split off for different environment files), how the requirements files are setup (whether they're split off for different environment files) and where those are located, and where static and dynamic media files are stored. As a reminder, the database configuration that was installed in Step 8 is:

    ```
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'django_db',
            'USER': 'django_login',
            'PASSWORD': 'secret',
            'HOST': '',
            'PORT': '',
        }
    }
    ```

13. Install they Python packages:

    a. Activate the Virtual Environment: `(ve) $ source /home/vagrant/.virtualenvs/djangoproj/bin/activate`

    b. Move to the `myproject` directory: `(ve) $ cd /vagrant/myproject`

    c. Install the requirements file: `(ve) $ sudo pip install -r ` and the name of the requiremnts file, wherever that lives

14. Update the `post-merge` Git hook to reference the correct requirements file (**NOTE** each person will have to do this since post-merge isn't part of the repo). That hook lives at `/vagrant/.git/hooks/post-merge`
15. Update the bash profile so that the `myproject` directory is opened by default when starting a new Terminal session. (**NOTE** each person will have to do this since the profile file isn't part of the repo): `(ve) $ echo "cd /vagrant/myproject" >> /home/vagrant/.profile`
16. Create and open for editing the Procfile and configure it to use the `frs` shortcut to simultaneously run `compass watch` and `runserver`: `(ve) $ vi /vagrant/myproject/Procfile.dev` and edit it to include:

    ```
    compass: compass watch static_media/stylesheets
    web: python manage.py runserver [::]:8000
    ```

17. Sync the database: `(ve) $ dj syncdb; dj migrate`
18. Smoke test: `(ve) $ frs` then open a browser window to [http://127.0.0.1:8001]()
19. Edit the bash profile to make sure the path to `static_media` in the shortcut for `cw` is correct. After editing, source the file: `(ve) $ source ~/.profile` and smoke test: `(ve) $ cw`.
20. Commit the project: `(ve) $ cd /vagrant && git add -A && git commit -am "initial commit"`
21. Create a new project on [BitBucket](http://bitbucket.com) or [GitHub](http://github.com) to host the new project and push following the directions from the Git host