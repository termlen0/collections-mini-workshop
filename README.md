# Goal:
The primary objective of this mini-workshop is to help gain an
understanding of working with Ansible collections.

## Pre-reqs:
- The user is familiar with Ansible and Ansible roles
- An environment with Ansible 2.9 and Ansible Tower 3.7 or greater installed
- A github account

### SECTION 1: Using a community collection from the command line

In this section you will learn how to use a collection from the
upstream Ansible Galaxy collection repository

##### STEP1: Create the necessary directory

Create a *demo-collection* directory in your home directory

``` bash
[student1@ansible-1 ~]$ mkdir demo-collection
[student1@ansible-1 ~]$ cd demo-collection/
[student1@ansible-1 demo-collection]$
```

This will be the base directory to build a playbook that uses an
upstream collection.


##### STEP3: Download the nginx collection

Navigate to https://galaxy.ansible.com and search of nginx. You should
see a result like:

![Alt Text](images/galaxy_nginx.png )

We will use the **nginx_core** collection. Click on it and review the
contents.

##### STEP4: Install the collection

The collection info page shows how to install the collection:

`ansible-galaxy collection install nginxinc.nginx_core`

Go ahead and execute this on your control machine:

``` bash
[student1@ansible-1 demo-collection]$ ansible-galaxy collection install nginxinc.nginx_core
Process install dependency map
Starting collection install process
Installing 'nginxinc.nginx_core:0.3.0' to '/home/student1/.ansible/collections/ansible_collections/nginxinc/nginx_core'
[student1@ansible-1 demo-collection]$
```

> NOTE: Note that the collection has been installed by default into
> the $HOME/.ansible path

##### STEP5: Use the downloaded collection

Collections are invoked by providing a FQCN (Fully Qualified
Collection Name) while accessing it from your playbook.

Create a playbook called *install_nginx.yml* to invoke the task from
the collection.

``` yaml

```


##### STEP6: Run the playbook

Execute the playbook : `ansible-playbook install-nginx.yml`

``` bash
[student1@ansible-1 demo-collection]$ ansible-playbook install-nginx.yml

PLAY [Install NGINX on node1] ****************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************
ok: [node1]

TASK [Install nginx] *************************************************************************************************************************************************************************************************

TASK [nginxinc.nginx_core.nginx : Check whether you are using a supported NGINX distribution] ************************************************************************************************************************
ok: [node1] => changed=false
  msg: Your OS, RedHat is supported by NGINX Open Source

TASK [nginxinc.nginx_core.nginx : Set up prerequisites] **************************************************************************************************************************************************************
included: /home/student1/.ansible/collections/ansible_collections/nginxinc/nginx_core/roles/nginx/tasks/prerequisites/prerequisites.yml for node1

TASK [nginxinc.nginx_core.nginx : Install dependencies] **************************************************************************************************************************************************************
included: /home/student1/.ansible/collections/ansible_collections/nginxinc/nginx_core/roles/nginx/tasks/prerequisites/install-dependencies.yml for node1

TASK [nginxinc.nginx_core.nginx : (Alpine Linux) Install dependencies] ***********************************************************************************************************************************************
skipping: [node1]

TASK [nginxinc.nginx_core.nginx : (Debian/Ubuntu) Install dependencies] **********************************************************************************************************************************************
skipping: [node1]

TASK [nginxinc.nginx_core.nginx : (Amazon Linux/CentOS/Oracle Linux/RHEL) Install dependencies] **********************************************************************************************************************
ok: [node1]

.
.
.
.
< truncated for brevity>
.
.
.
RUNNING HANDLER [nginxinc.nginx_core.nginx : (Handler) Start/reload NGINX] *******************************************************************************************************************************************
changed: [node1]

RUNNING HANDLER [nginxinc.nginx_core.nginx : (Handler) Check NGINX] **************************************************************************************************************************************************
ok: [node1]

RUNNING HANDLER [nginxinc.nginx_core.nginx : (Handler) Print NGINX error if syntax check fails] **********************************************************************************************************************
skipping: [node1]

TASK [nginxinc.nginx_core.nginx : Debug NGINX output] ****************************************************************************************************************************************************************
skipping: [node1]

TASK [nginxinc.nginx_core.nginx : Configure logrotate for NGINX] *****************************************************************************************************************************************************
skipping: [node1]

TASK [nginxinc.nginx_core.nginx : Install NGINX Amplify] *************************************************************************************************************************************************************
skipping: [node1]

PLAY RECAP ***********************************************************************************************************************************************************************************************************
node1                      : ok=14   changed=4    unreachable=0    failed=0    skipped=23   rescued=0    ignored=0


```



### SECTION 2: Using a community collection from Ansible Tower


In this section, we'll see how easy it is to use this playbook
from Ansible Tower.


##### STEP1: Galaxy Settings
Review the Galaxy configuration in Tower. Login as an admin into Tower
and Navigate to *Jobs>Settings* on your Tower.




> NOTE: This is where you would configure your enterprise
> automation hub settings


##### STEP2: Review the source code for the Ansible Tower Project

Navigate to



#### Task 1: Create an Ansible Galaxy account

For this workshop, we will host the collection you create on
https://galaxy.ansible.com. This is considered an *upstream*
repository for user content and is world readable. For enterprise
applications you would be using the private automation hub, which can
be hosted on premises or in any secure compute environment
(private/public clouds etc).

##### STEP 1:
Navigate to https://galaxy.ansible.com and create a galaxy account
using your github account.

On the left menu, click on **My Content**. This will reveal all the
namespaces you have access to.
![namespaces](images/galaxy_namespaces.png )


##### STEP 2:
Navigate to your username (top-right) and click on preferences.
![pref](images/galaxy_preferences.png ). Verify your email.

Also click on the **Show API** button and save that information
locally on your laptop somewhere. You will need this in a later step.

#### Task 2: Fork and Clone this repository

Click on the *Fork* button to fork this repository under your own github.

Clone this repository onto your control machine.

``` bash
[student1@ansible-1 ~]$ git clone https://github.com/<your_user>/collections-mini-workshop
Cloning into 'collections-mini-workshop'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 200 bytes | 200.00 KiB/s, done.

[student1@ansible-1 ~]$ cd collections-mini-workshop/
[student1@ansible-1 collections-mini-workshop]$

```

> You should see an old-role directory that has an existing playbook
> that invokes multiple roles. You will not execute this.
As we discussed, collections are a new way of packaging content, in
> the Ansible Automation platform. We also spent time to understand
> the value of this packaging format.
A common situation that users encounter is that they have already
> invested time and effort and already have roles that are being used
> in their playbooks and they might want to refactor some of these
> roles as collections.
In this section of the mini-workshop, you will simply refactor these
> existing roles as community collections and then learn how to use
> them with Ansible Tower.

STEP1: Initialize a collection

``` bash
$ ansible-galaxy collection init --init-path mycollections termlen0.acme_collection

- Collection termlen0.acme_collection was created successfully
```

> NOTE: Replace termlen0 above, with your own galaxy namespace.

Switch to the acme_collection directory:

`$ cd mycollections/termlen0/acme_collection/`

Let's take a look at the directory structure and files that were
auto-generated:

``` bash
$ tree
.
├── docs
├── galaxy.yml
├── plugins
│   └── README.md
├── README.md
└── roles

3 directories, 3 files

```

A short description of the collection structure:

- The plugins folder holds plugins, modules, and module_utils that can
  be reused in playbooks and roles.

- The roles folder hosts custom roles, while all collection
  playbooks must be stored in the playbooks folder.

- The docs folder can be used for the collections documentation, as
  well as the main README.md file that is used to describe the
  collection and its content.

- The tests folder holds tests written for the collection.

- The galaxy.yml file is a YAML text file that contains all the
  metadata used in the Ansible Galaxy hub to index the
  collection. It is also used to list collection dependencies, if
  there are any.

>NOTE:
When a collection is downloaded with the ansible-galaxy collection install two more files are installed:

    MANIFEST.json, holding additional Galaxy metadata in JSON format.

    FILES.json, a JSON object containing all the files SHA256 checksum.


Ansible collections support semantic versioning (linky).
 Every time the collection is updated, the galaxy.yml
file has to be updated with a version number.

STEP 2: Move the old roles into the new collection data structure

Simply copy over the old roles into the new collection's roles
directory:


``` bash
$ cp -r ../../../old-role/roles/* roles/
$ tree -L 2
.
├── docs
├── galaxy.yml
├── plugins
│   └── README.md
├── README.md
└── roles
    ├── apache_vhost
    └── firewall

5 directories, 3 files

```
