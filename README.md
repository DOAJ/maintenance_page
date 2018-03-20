# maintenance_page
A page to show when the main DOAJ site is down.

## Deployment

This is managed using git hooks. After you have checked out the repo,
add the ```post-receive``` hook to your local git directory:

    ln -sf deploy/hooks/post-receive .git/hooks/post-receive

You'll also need the server in your ```.ssh/config``` file, and the remote:

    git remote add origin cloo@doaj-maintenance-page-webhost:maintenance_page.git
    
where ```maintenance_page.git``` has already been created with ```git init --bare```.

You can only deploy the master branch. To do so, run the following command:

    git push production master
    
i.e. you are pushing master to the production remote. This will run the hooks, and
restart ```nginx``` for you.