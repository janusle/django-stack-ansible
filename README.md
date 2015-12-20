# Django-stack-ansible

The ansible playbook is used to deploy most of my personal django projects

It deploys:

* Nginx
* uWSGI
* Django
* Postgresql(Optional)

It creates:

```
srv
└── www
    └── {{ hostname }}  # example: example.com
        ├── code     <---- Django project
        ├── envdir   <---- environment variable files
        ├── log      <---- Django logs
        ├── nginx    <---- Nginx configuration file for the site
        ├── static   <---- Static files(for manage.py collectstatic)
        ├── uwsgi    <---- uWSGI related files
        │   ├── etc  <---- uWSGI configuration file for the site
        │   └── sock <---- uWSGI socket files
        └── venv     <---- virtualenv
```

## Deployment Requirements

+ [Ansible](http://www.ansible.com/) >= 1.9.4


## Before Deployment

1. Create inventory files under ansible/environments

    Two example files([production_example](ansible/environments/production_example) and [vagrant_example](ansible/environments/vagrant_example)) can be found in ansible/environments.

2. Create a var file under ansible/group_vars

    An [example](ansible/group_vars/web.sample) can be found in ansible/group_vars.

3. Create a var file under ansible/host_vars

    An [example](ansible/host_vars/host_sample) can be found in ansible/host_vars.

4. Encrypt var files under ansible/host_vars(The reason for encryption is host var files contains sensitive data like credentials)

    ```bash
    # Place password into a file
    echo "VERY SECURE PASSWORD" > .ansible-vault-file

    # Encrypt var file. Replace FILE_NAME with your host var file name
    ansible-vault encrypt ansible/host_vars/FILE_NAME --vault-password-file=.ansible-vault-file
    ```

5. Set up ssh config for the server if needed.

    A sample ssh config is like:

    ```
    Host server1
      HostName x.x.x.x
      User xxx
      IdentityFile ~/.ssh/ssh_key
    ```

## Deployment

```bash
# Replace INVENTORY_FILE with your inventory file name
ansible-playbook -i environments/INVENTORY_FILE --vault-password-file=.ansible-vault-file site.yml
```
