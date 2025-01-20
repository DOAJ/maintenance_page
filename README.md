# maintenance_page
A page to show when the main DOAJ site is down.

NEW: This can now be deployed via ansible in the sysadmin repo.

## Deployment

This is managed using git hooks. You'll need the server in your ```.ssh/config``` file,
and the remote:

    git remote add production cloo@doaj-maintenance-page-webhost:maintenance_page.git

You can only deploy the master branch. To do so, run the following command:

    git push production master
    
i.e. you are pushing master to the production remote. This will run the hooks, and
restart ```nginx``` for you.

## Server configuration

You need a bare git repo on the host, called ```maintenance_page.git```. This is created
with ```git init --bare maintenance_page.git``` in the home directory.

Create the working tree directory where nginx will serve the files from and fix the
permissions:

    sudo mkdir /var/www/maintenance_page
    sudo chown www-data:www-data /var/www/maintenance_page
    sudo chmod -R 775 /var/www
    sudo usermod -a -G www-data cloo

The post receive hook then needs to be created on the host, by copying
```deploy/hooks/post-receive``` to ```~/maintenance_page.git/hooks/post-receive``` and
```chmod +x hooks/post-receive``` on the host. This will allow the script to run on
deploy and copy the code to the correct work tree.

Afterwards, you can remove the copied hook and replace it with a symlink to the checked
out hook (so you can update the hooks):

    ln -sf /var/www/maintenance_page/deploy/hooks/post-receive hooks/post-receive
    chmod +x hooks/post-receive
    
## SSL Certificates

The nginx config expects an SSL certificate - this should be restored from a backup.

## Using Git on the machine

Because we have a different work tree set, we need to supply these to any git command, e.g.:

    git --work-tree=/var/www/maintenance_page --git-dir=/home/cloo/maintenance_page.git status
or
    git --work-tree=/var/www/maintenance_page --git-dir=/home/cloo/maintenance_page.git reset --hard
