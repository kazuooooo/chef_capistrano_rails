chef & capistrano(with unicorn) practice project.

you can build rails environment on centOS, and deploy sample rails project to it.

### Clone Submodule
```
git submodule init
git submodule foreach git pull origin master
git submodule update
```

### Vagrant setup

```
$ vagrant box add bento/centos-7.1
```

```
$ vagrant init bento/centos-7.1
```

then edit Vagrantfile Edit
```
~
config.vm.network "private_network", ip: "192.168.33.30" 
~
```

then
```
vagrant up
```

add host to vagrant

```
$ vagrant ssh-config --host rails_env >> ~/.ssh/config
$ ssh rails_env #check connection
[vagrant@localhost ~]$
```

### Make Rails environment using Chef

First, bundle install
```
cd rails_env_chef
bundle install --path vendor/bundle
```

```
bundle exec knife configure # all enter
```

Before cook, you have to put .secret directory to rails_env_chef/
(this is not on git repo)
```
mv .secret git_repo/rails_env_chef/.secret
```

Then,
```
bundle exec knife solo prepare rails_env
bundle exec knife solo cook rails_env
```

### Deploy App using Capistrano

First, bundle install
```
cd sample_project
bundle install --path vendor/bundle
```

Then, deploy sample_project to the server
```
bundle exec cap production deploy
(need enter pass:vagrant(default))
```

### Check

Try access to 192.168.33.30(ip you set in Vagrantfile) using browser

If you see this rails welcomewindow window, it succeed to deproy.
