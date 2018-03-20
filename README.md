# maintenance_page
A page to show when the main DOAJ site is down.

## Deployment

This is managed using git hooks. You'll need the server in your ```.ssh/config``` file,
and the remote:

    git remote add origin cloo@doaj-maintenance-page-webhost:maintenance_page.git

You can only deploy the master branch. To do so, run the following command:

    git push production master
    
i.e. you are pushing master to the production remote. This will run the hooks, and
restart ```nginx``` for you.

## Server configuration

You need a bare git repo on the host, called ```maintenance_page.git```. This is created
with ```git init --bare maintenance_page.git``` in the home directory.

The post receive hook needs to be created on the host, by copying ```deploy/hooks/post-receive```
to get the code in the working directory, then symlinking the checked out hook:

    ln -sf /var/www/maintenance_page/deploy/hooks/post-receive hooks/post-receive
    chmod +x hooks/post-receive