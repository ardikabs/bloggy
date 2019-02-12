# Blog CLI Suites
It is a CLI Application that have feature for automate a process from provision an instance until deployed a blog site with Wordpress.

## Dependencies
1. Terraform version: 0.11.11
2. Ansible version: 2.7.7
3. Python version: 3.6.7
4. Operating System: Only __LINUX__

## How to install
1. Clone this [repo](https://github.com/ardikabs/blog-automation-terraform-ansible-stack.git).
2. Make sure you already have an installed python3.6, if not, follow this a guide to install python3.6.
   * [Ubuntu](https://askubuntu.com/questions/865554/how-do-i-install-python-3-6-using-apt-get)
   * [CentOS](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-centos-7)
3. Also we need to install `pip3`, then we install an `virtualenv`.
4. We are using `virtualenv` to used the ansible with python3 as the interpreter. Just use this command to install ansible, `pip install ansible`
5. To install the CLI Apps, make sure you already activate `virtualenv` and in the same directory of this repo, then used this command to install the CLI apps - `pip install cli/`
6. In case, if you dont want to install the CLI Apps, we already make an entrypoint script (`blog-entry.py`), before use you need to install the dependencies, use this command (`pip install -r blog-requirements.txt`)
7. For Ansible, the default configuration file already exists in the directory `ansible`, but in case having problem use this command `export ANSIBLE_CONFIG=$PWD/ansible/ansible.cfg` or you can just copy that configuration file to your ansible configuration file.
8. For Terraform, use this guide to install [Terraform](https://www.terraform.io/downloads.html).
9. We are strongly recommend to use `virtualenv` before use this CLI Apps.

## How to use the CLI
In the CLI apps, we have the list of the feature in our CLI.
__Attention!__ Consider to check the configuration file first before used, otherwise you will caught an error.
1. `blog setup` - This command will do the provisioning of the server instances beside the provisioning, this command will create a swarm cluster and automatically joing for the workers to the managers. This process done by the Terraform and Ansible.
2. `blog up` - This command will do create an instance of Database MySQL first then the Wordpress blog site instances. The wordpress blog site, can be access in this url `http://SERVER_IP:8000`. This command done by the help of Python Fabric.
3. `blog scale` - This command will do the scale up for the wordpress instances.
4. `blog status` - This command, only checking the number of existing wordpress instances.
5. `blog down` - This command will do the destroying all the instances including the server instances

All the command, have the ability for debug all the process with the options `-d`. All the configuration to be used are ready in the file `blog.cfg`.

-- The thing that need to be changed in configuration file - `blog.cfg`
`state` - This is the state that used to track the server instances, you need to set the state file path with absolute path, otherwise this CLI Apps can track the process even start the wordpress instances.
`ssh_private_key` - This is used by Ansible and also Terraform. A SSH Private Key path.
`ssh_public_key` - This is used by Ansible and also Terraform. A SSH Public Key path.
`token` - This is `DIGITALOCEAN_TOKEN`, you need to change this.

### Documentation
You can see the documentation the screenshot in the `img/` directory.

#### Blog Setup Process
![Blog Setup Process](https://media.giphy.com/media/9u1bsLJuN5it4LTn01/giphy.gif)

#### Blog Up Process
![Blog Up Process](https://media.giphy.com/media/2UxQj3yghxW1FBjDVz/giphy.gif)

#### Blog Scale Up Process
![Blog Scale Up Process](https://media.giphy.com/media/iOFNT8mf68nAMzMdDp/giphy.gif)

#### Blog Scale Down Process
![Blog Scale Down Process](https://media.giphy.com/media/8vvZLTmVbQtILyIb5N/giphy.gif)

#### Blog Down Process
![Blog Scale Down Process](https://media.giphy.com/media/5t0xBWIEOE3wzJyQw4/giphy.gif)


### Summary
We already design this with the assumption of proof of concept that we can automate the process from provisioning until the Wordpress blog site is ready to accept the user request. So, in the default configuration, we will create a swarm cluster with *one managers* and *one workers*. 

The database of MySQL will be deployed on the managers, for the rest will be the workers. So this database will be the one and only database to be used for all the Wordpress instance in our Wordpress blog site.

For the persistant volume, we dont care about this for now. So instead using persistant volume, we will only use the docker volume mounting on each side (the managers and workers) to take care the static file of the Wordpress blog site.

All the instance are used in SINGAPORE region (sgp1) in [DigitalOcean](https://www.digitalocean.com/).
