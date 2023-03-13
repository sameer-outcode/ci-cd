## Ansible Instruction
Firstly make sure you are able to connect to remote server via ssh. Then you can run ansible script to install rails and postgres and create postgres user. 
Here ansible consists of roles and inventory. Roles is ansible is created using ansible galaxy which is used to refactor and make ansible reusable. 
Run following commands to run ansible<br> 

`sudo apt-add-repository ppa:ansible/ansible`<br>
`sudo apt update`<br>
`sudo apt install ansible`

Then hosts file is created which is called inventory. Inventory consists of target servers where we want to run ansible scripts. 

To check if the ansible is working or not, we can ping to the remote server using following command. <br>

`ansible -i <HOSTFILE PATH> -m ping`

It ensures the connectivity with the hosts. 

Finally run ansible command using following command. 

`ansible-playbook -i <HOSTFILE PATH> <PLAYBOOK YML PATH>`

## Packages installed for rails environment
 Using package module
 * git and curl <br>

 Using shell module
 * rbenv, ruby build and ruby.

 Using Roles
 * Postgres

## Note
By default ansible has sh shell. Since there are few contrast on commands on shells, I used bash shell for ansible. 

### NOTE: If you need more roles, you can run ansible galaxy to create your custom roles. 

## CircleCI Instruction
CircleCI is the continuous integration & delivery platform that helps the development teams to automate the build, test, and deployment of their codes. CircleCI can be configured to run very complex pipelines efficiently with caching, docker layer caching, resource classes, and many more.
Few steps to follow before writing circleCI. 
* Login to circleCI using any github/bitbucket account. 
* Create .circleci directory and create config.yml file inside it where you will write your jobs and workflows you need for CI/CD.
* Then choose the repo where your workflows will run.

In the config file, we have two jobs naming build and deploy. 

### Action in build job
* Pulled ruby docker image
* Pulled postgres docker image and mapped ports with the local container to connect with postgres
* Created postgres db, username, password and configure the same settings on test db on database.yml

        test:
          <<: *default
          database: ci_cd_test
          username: postgres
          password: postgres
          host: localhost

* Then few steps are run to install dependencies required like bundler, postgres-client etc. 
* Then run rubocop to check ruby syntax and rspec to run test. 

### Action on Deploy Job. 
* Pulled ruby docker image
* Installed bundler and run `bundle exec cap production deploy` to deploy

Finally workflows is created where both jobs are run sequentially. 

### Few things to note
* Deploy job is run after build job runs successfully as deploy job requires build job. 
* Inorder to deploy to production, `Capistrano` gem has been used which is a deployment tool. 
* In order to push to production or for the connectivity with remote server, you need to add ssh keys of your local machine on circleCI.
    * Go to Project Settings.
![2023-03-13_15-45.png](..%2FDesktop%2F2023-03-13_15-45.png)
  * Go to Add SSH and add your private key via which you are authorized to the remote server.
![2023-03-13_15-47.png](..%2FDesktop%2F2023-03-13_15-47.png)

![2023-03-13_15-48.png](..%2FDesktop%2F2023-03-13_15-48.png)

## Github Actions 
Additionally you can use github actions instead of circleCI to for CI/CD. 
The workflows are similar to github action except we need to add ssh key via ssh agent on github action on your runner. 
For that follow few steps below: 
* Go to Settings > Secrets and Variables > Actions 
* Click on new repository secret to add your private key which is authenticated on remote server. Add as environment variable which you will be fetching on github action. 



  





