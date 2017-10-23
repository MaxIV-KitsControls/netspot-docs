# NetSPOT Web GUI

This is the web interface for NetSPOT. It's served from **w-v-netconf-0**.

## Test server

Follow the instructions in the server setup page. Make sure the settings points to a different database than the production database.

To run a development server run this command in the netspot directory:

    /usr/bin/python manage.py runserver 0.0.0.0:8000

## Change and commit a change

Make sure you have the latest version

    git pull

Do your changes and then commit them to the repo.

    git status                    # This will list all the files you've changed.
    git diff the_file_I_changed   # Presents a diff, if happy proceed
    git add the_file_I_changed    # Repeat for all the files you wish to change and commit
    git status
    git commit -m 'Description of my change here.'
    git push origin master

That's it! Don't forget to push the new version of the web interface to the automation server.

## Push a new version

To push a new version of the web gui run the following playbook: update_netspot.yml.

The playbook is located in the NetSPOT installation. The easiest way to do this is this:

    git clone git@github.com:MaxIV-KitsControls/netspot.git
    cd netspot

And then:

There are instructions in the file of how to run it. But basically login to the automation server (**w-v-netconf-0**) and run this:

    ansible-playbook netspot/lib/network_playbooks/update_netspot.yml --ask-sudo-pass

## Add a new playbook to the web interface

Submit you new playbook to the NetSPOT repo. Then:

Go to [NetSPOT admin](http://netspot/admin/netspot/playbook/) and click "Add Playbook." Fill in all the fields and that's it. The playbook should now be available in the NetSPOT webgui.
