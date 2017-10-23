# NetSPOT Documentation

This describes the NetSPOT documentation aka this documentation.

## Clone the repo

In you home directory:

    git clone git@gitlab.maxiv.lu.se:olokas/spotmax-docs.git
    cd spotmax-docs/

## Change and commit your changes

Do the changes and then run the following commands:

    git status                        # List all files changed
    git diff MY_FILE              # Get a diff of you change
    git add MY_FILE
    git commit -m 'MY COMMIT DESCRIPTION HERE'
    git push origin master

## Push the documentaion changes to the automation web server

SSH to the automation server and update the server (need to clone the playbook repo first):

        ssh w-v-netconf-0
        ansible-playbook network_playbooks/update_netspot.yml -i hosts --ask-sudo-pass

