About the project

# Project Scope
Create an ansible playbook that does the following:
- Installs Apache 2.4 ( not the latest, but a specific version - you can choose which)  
- Create a VHost and activate it  
- Create a redirect mapping file that contains 5 redirect examples( has to be designed for the above VHost)

# Basic setup

**Control Node**
- Used as control node that schedules work based on ansible playbooks to other worker nodes.
- Runs Debian9
	- Output of `lsb_release -a`:
		- `Distributor ID:	Debian
		Description:	Debian GNU/Linux 9.13 (stretch)
		Release:	9.13
		Codename:	stretch`
- Has latest ansible version installed:
	- Ouput of `ansible --version`:
		- `ansible 2.2.1.0`
- Enabled passwordless login for the user `ansible` on remote worker node using `ssh-keygen` and `ssh-copy-id`.
- Has all ansible configuration and playbook files.


**Worker Node**
- Carries out tasks given by control node through ansible playbooks.
- Runs Ubuntu18.04
	- Output of `lsb_release -a`:
		- `Distributor ID:	Ubuntu
		Description:	Ubuntu 18.04.5 LTS
		Release:	18.04
		Codename:	bionic` 
 - SSH Password Authentication is disabled by modifying relevant directives in `/etc/ssh/sshd_config`
 - Has an ansible user with sudo privileges used to carry out tasks on the server.
	 - ansible user sudo privileges achieved by editing the sudoers file `/etc/sudoers` and adding `ansible ALL=(ALL) NOPASSWD: ALL`
		 - can also be achieved with `sudo usermod -a -G sudo ansible`


## Project Structure

<pre>
ansible_project/
|-- apache_playbook.yml
|-- config_files
|   `-- apache2.conf.j2
|-- project_inventory.txt
|-- README.md
|-- redirects
|   `-- redirect_map.txt
|-- vars
|   `-- defaults.yml
`-- web_templates
    |-- index.html.j2
    |-- new_links
    |   |-- prod_id_11.html
    |   |-- prod_id_12.html
    |   |-- prod_id_13.html
    |   |-- prod_id_14.html
    |   `-- prod_id_15.html
    `-- old_links
        |-- blackberry.html
        |-- galaxy.html
        |-- iphone.html
        |-- oneplus.html
        `-- pixel.html

6 directories, 17 files
</pre>

**apache_playbook.yml**
- Main ansible playbook file consisting of all the tasks needed to achieve the project scope

**apache2.conf.j2**
- Config file used to configure the apache2 virtual host using Jinja2 templating language

**project_inventory.txt**
- Main inventory used by ansible in this project
- Currently just contains a group of [clients] and the IP of the worker node

**redirect_map.txt**
- The redirect mapping file that contains 5 redirect examples

**vars**
- Directory with 1 file used to define several ansible vars used in various template files.

**web_templates**
- `index.html.j2` base html file used to display when accessing the root server document.
- `new_links` and `old_links` are 2 directories containing various html files used to demonstrate the redirect requirement of the project scope.

## Usage
Using the current setup just run the following on control node:
`ansible-playbook -i project_inventory.txt apache_playbook.yml`

## Notes
The project can be further improved depending on additional requirements.

Additional improvements i see fit:
- Generalizing the installation and configuration of services based on the distribution ansible tasks run against using ansible facts.
- Improving the apache2 configuration.
	- Hardening
	- Enabling compression

