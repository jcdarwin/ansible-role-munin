ansible-role-munin
==================

An Ansible role, available via [Ansible Galaxy](https://galaxy.ansible.com), that installs and configures munin on Ubuntu.

This role works with [our repo to create a vagrant virtualbox vm](https://github.com/jcdarwin/ansible-roles-vagrant), but is easily modifed to work with actual ubuntu boxes.

You'll probably want to amend / override:

* `defaults/main.yml`

Installation
------------

Preusming a `requirements.yml` as follows:

    # Install a role from GitHub
    - name: ansible-role-munin
    src: https://github.com/jcdarwin/ansible-role-munin

We can install the role locally, using a `requirements.yml` file:

    # Install a role from GitHub
    - name: ansible-role-munin
    src: https://github.com/jcdarwin/ansible-role-munin
    path: roles/

Install the role:

    ansible-galaxy install -r requirements.yml -p ./roles

Requirements
------------

You may already have:

* Created a vagrant VirtualBox virtual machine using [ansible-roles-vagrant](https://github.com/jcdarwin/ansible-role-users)
* Provisioned the virtual machine with users using [ansible-role-users](https://github.com/jcdarwin/ansible-role-users)
* Installled various common packages using [ansible_role_common](https://github.com/jcdarwin/ansible-role-common)
* Installed and configured MySQL using [ansible_role_mysql](https://github.com/jcdarwin/ansible-role-mysql)
* Installed and configured PHP using [ansible_role_php](https://github.com/jcdarwin/ansible-role-php)
* Installed and configured Apache using [ansible_role_apache](https://github.com/jcdarwin/ansible-role-apache)
* Installed and configured Monit using [ansible_role_monit](https://github.com/jcdarwin/ansible-role-monit)

Role Variables
--------------

Available variables are listed below, along with default values as found in `defaults/main.yml`:

    site:
      app: site
      host: localhost.vagrant
      user: administrator
      password: whatever

    ansible_role_munin:
      hostname: munin.{{ site.host }}
      docroot: /var/cache/munin/www
      user: "{{ site.user }}"
      password: "{{ site.password }}"

Dependencies
------------

None.

Example Playbook
----------------

Our `hosts` file, as generated by [our repo to create a vagrant virtualbox vm](https://github.com/jcdarwin/ansible-roles-vagrant):

    [vagrant]
    host1 ansible_ssh_host=127.0.0.1 ansible_ssh_port=2222 ansible_user=vagrant ansible_ssh_private_key_file=../.vagrant/machines/default/virtualbox/private_key

We include a playbook at `main.yml`.

Running the playbook:

    # Note that we're presuming our hosts file has been generated by our vagrant repo
    ansible all -m ping -i ../vagrant/ansible/hosts -l all

    ansible-playbook -l all main.yml -i ../vagrant/ansible/hosts --extra-vars "scheme=http"

    # Check that munin has been installed
    ansible -m shell -a 'sudo munin-node-configure --shell' all -i ../vagrant/ansible/hosts

If you want to enable ssl using certbot:

    # Manually, on the server:
    /opt/certbot/certbot-auto certonly --webroot -w /var/cache/munin/www/ -d munin.whatever.co.nz

    # This should result in a cert installed at:
    /etc/letsencrypt/live/munin.whatever.co.nz/fullchain.pem

    # Run our playbook to install munin using port 80 and port 443
    ansible-playbook -l all main.yml -i ../vagrant/ansible/hosts --extra-vars "scheme=https"

To see the results in the browser (presuming you're using the defaults):

	# Add the subdomain to our hosts file
	echo "127.0.0.1	munin.localhost.vagrant"

	# then visit: http://munin.localhost.vagrant/

License
-------

MIT

Author Information
------------------

http://github.com/jcdarwin
