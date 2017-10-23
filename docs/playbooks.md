# Playbooks

The Ansible playbooks are located in **https://gitlab.maxiv.lu.se/olokas/network_playbooks**.

## Clone the playbook repo

In your home directory:

    git clone git@github.com:MaxIV-KitsControls/netspot.git
    cd netspot/

Playbooks are located in *netspot/lib/network_playbooks*.

## Add a new playbook

Create a new playbook file. The file needs to be a **.yml** file. Then run the following commands to add the new playbook to the repo:

    git add my_playbook.yml
    git commit -m 'My new playbook.'
    git push origin master

Please note that you need to configure the web gui to pick up the new playbook. See **Configure the web GUI to pick up a new playbook** for more details.

## Change and commit your changes

Do the changes and then run the following commands:

    git status                        # List all files changed
    git diff MY_PLAYBOOK              # Get a diff of you change
    git add MY_PLAYBOOK
    git commit -m 'MY COMMIT DESCRIPTION HERE'
    git push origin master

## Pull the latest changes on the automation server

In order to update NetSPOT with the new playbook you need to update the server. Please see "Server Setup" for instructions.

## Configure the web GUI to pick up a new playbook

Please see [Add a new playbook to the web interface](netspotweb)
