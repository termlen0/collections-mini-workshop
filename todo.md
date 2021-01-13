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
