Overview
========

This is my personal website. It's hosted on OpenShift because Heroku isn't free.

OpenShift Setup
---------------

Sign up on OpenShift.

Copy the `.openshift` directory from [https://github.com/openshift/rails4-example](https://github.com/openshift/rails4-example) into app (already have it here). That directory contains all necessary deployment and build scripts.

Create an app online at OpenShift. Choose to create a Ruby 2.0 application. You can customize settings there, but if you don't care to customize, you can instead run the command below. If you customize online, remember to add a Postgresql cartridge.

    rhc app create myapp ruby-2.0 postgresql-9.2

Add remote to your git remote.

    git remote add openshift ssh://554blahblahblah5000133@myapp-jackchan.rhcloud.com/~/git/myapp.git/

Set the env var so that OpenShift doesn't install development and test gems.

    rhc env set BUNDLE_WITHOUT="development test" --app myapp

Push your app to OpenShift.

    git push openshift master # git push -f openshift master when pushing first time

SSH into OpenShift machine and install `rack`.

    rhc ssh web
    cd app-root/
    gem install rack # version 1.6.4 worked for me
    exit

Get the backup of your previous database. For me, I had to get it off Heroku, so first get the public URL of where the backup is located.

    heroku pg:backups public-url b001 --app my-heroku-app # copy the output (a super long URL)

Now actually download the backup from Heroku.

    curl -o yourdbbackup.dump "https://the.urlfromprev.io/usstep"

Push the database backup file up to OpenShift. Open a new terminal window.

    rhc port-forward myapp # leave the window open

Back in the main terminal window, execute the following command. Note that `yourdbbackup.dump` is the database backup file, like if you pulled the backup from Heroku. You can find your admin user name, password, and database table name by logging into OpenShift and clicking on your app.

    pg_restore --verbose --clean --no-acl --no-owner -h localhost -p 5432 -U youradminusername -d myapp yourdbbackup.dump
    # now enter the password

The web app should now work. Note that every time you push, assets will be compiled

Useful OpenShift Commands
-------------------------

Stream logs

    rhc tail myapp

List env vars

    rhc env list myapp

Restart web app

    rhc app restart myapp

SSH into your OpenShift app

    rhc ssh myapp

Port forward your app so that you can access postgresql

    rhc port-forward myapp

Download database backup

    # Forward port in a separate terminal window, note the port for postgresql
    pg_dump -h localhost -p <postgresql port> -U youradminname -F custom -f backup.dump myapp # 'custom' is for psql format, 'plain' is for plaintext format