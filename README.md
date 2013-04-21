Project Overview
====================

This project is the foundation for our personal websites. The website will have a very basic content management system (CMS) that allows for easy content publishing so that we don't have to bust out terminals and text editors every time we want to post some content to our websites. 

The CMS will require a login system (although we only need to have one user in the end). Once this is implemented, we can take this project and customize front-end or whatever else for our own personal websites.

Dev Environment Setup
====================

If not running on native Ubuntu, first install a VirtualBox, create a new virtual machine, and  then install Ubuntu 12.10. 

To have a responsive VirtualBox system, give Ubuntu 1024 MB (or more) of RAM, enable 3D acceleration, and install VirtualBox's Guest Additions.

Once Ubuntu is set up, here are a few things that you'll need to do in order for Rails to work as we want.

Open terminal, go to edit -> profile preferences -> Title and Command tab -> check the "Run command as a login shell" box.

	# installs bunch of packages that are needed later
	sudo apt-get install zlib1g-dev curl sqlite3 libsqlite3-dev nodejs openssl 
	
	# install RVM with Ruby
	\curl -L https://get.rvm.io | bash -s stable --ruby 
	
	# runs script to configure Ruby
	source /home/jchan/.rvm/scripts/rvm

May need to remove ruby 1.9.3 and reinstall b/c of openssl. if so, run the two lines below:

	# remove ruby 1.9.3
	rvm remove 1.9.3

	# install & compile w/ openssl
	rvm install 1.9.3 --with-openssl-dir=$HOME/.rvm/usr
	
Use Ruby 1.9.3 (run this just in case you're running another version) 

	rvm use 1.9.3 --default

Check Ruby version to see that you're using 1.9.3

	ruby -v
	
Check gem version (shows that gem is installed)

	gem -v
	
Install Rails

	\curl -L https://get.rvm.io | bash -s stable --rails
	
Check Rails version (shows that Rails is installed)

	rails -v 
	
Run rvm configuration script

	source /home/jchan/.rvm/scripts/rvm

Now install heroku toolbelt:

	wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh 

###Post Setup
Check that rails has no problems by making it create a new rails app:

	rails new abcdefg

This should create a folder abcdefg with bunch of files inside. If this ran sucessfully, rails has no problems. You can now use rm -r to delete that folder.

Now check that everything for our project works. Go to the directory where the # project is and then run:

	bundle install --without production 

The line above will install project's dependencies without the postrgesql package that's used only on heroku's end (causes local problems if using something besides PostgreSQL on development database)

Try to start a localhost server

	rails s

If there are no errors, everything should work.
Open up a web browser and navigate to http://localhost:3000/
You ought to see the website.

###Git Settings
To push to Github without entering username and password every time, make sure to configure the remote this way:

	git remote set-url origin git@github.com:jchan172/website_base.git

You have to upload your SSH key to Github. If you don't have an SSH key, run this line:

	ssh-keygen-t rsa -C "your_email@example.com"

Then enter a passphrase (don't need to use passphrase, so press enter and then enter again)

Your SSH keys are now in ~/.ssh/

Login to Github, click on your username, click on "Edit your profile", and then click on "SSH Keys" on the left.

Click on the "Add SSH key" and paste the contents from ~/.ssh/id_rsa.pub

Test to see if this works by running this:

	ssh -T git@github.com

Remember to set name and email so that it shows up in commits

	git config --global user.name "Your Name"
	git config --global user.email your@email.com


###Git Tools
git-cola is a nice all-in-one tool.

	sudo apt-get install git-cola

gitg is nice for examining history.

	sudo apt-get install gitg

All you have to do is run either of these tools inside the project directory, and a nice GUI will pop up.

I'll be using git-cola to commit, push, and doing any branch stuff, and I'll use gitg for visualizing history.