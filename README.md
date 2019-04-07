### Cygate Techdays 2019
Ansible automation demo for Cygate Techdays 2019 by Christofer Tibbelin
## websiteX role :blue_book::green_book::orange_book:
### a simple role that installes apache2 and installs a simple webpage
#### It uses both copy files and also templates to do this.
> The Role installes apache2 on ubuntu and copies one http.conf file from files. It also uses >templates to create a new file for index.html
> It will restart the service if there have been a change using the handlers
#### Files and folder structure
```sh
websiteX/
├── defaults
│   └── main.yml
├── files
│   └── apache2.conf
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── README.md
├── tasks
│   ├── configure.yml
│   ├── install.yml
│   ├── main.yml
│   └── service.yml
├── templates
│   └── index.html.j2
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
#### Requirements
that the server is a ubuntu, as I don't check for the operating system. it will fail if not ubuntu.
That you are sudo user on the host.
#### Role Variables
example default varible uses for index.html
website_name = "Techday Web"
#### Dependencies
no dependencies
#### Example Playbook
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yml
- hosts: servers
  roles:
     - { role: websiteX }
```
#### License
MIT
#### Author Information
Christofer Tibbelin with help of Internet, Google and everyone else.

## How we created this role:
> "ansible-galaxy init" creates a starting folder structure for a role.\
> I'm creating a role called websiteX and that it's my own with the --offline argument.
```sh
ansible-galaxy init websiteX --offline
```
> You can remove the stuff you don't need from this folder structure
```sh
websiteX
├── defaults #Default variables
│   └── main.yml #main.yml is the startfile in each folder
├── files #Files you need in your role that you don`t change.
├── handlers #Tasks that will only be run once. ex service restart
│   └── main.yml #Ansible always start reading main.yml in each folder
├── meta #Metadata about this role
│   └── main.yml #Ansible always start reading main.yml in each folder
├── README.md #Description of the role
├── tasks #All tasks you have for this role.
│   └── main.yml #Ansible always start reading main.yml in each folder
├── templates #Templates using jinja2 template language. Like files but can be modified
├── tests #If you have automatic testing in your role
└── vars #Variables that will override the defaults.
    └── main.yml #Ansible always start reading main.yml in each folder
```
#### My role websiteX should install and configure a server to be my public website for our DMZ
> Default location for roles is either /etc/ansible/roles or ~/.ansible/roles\
#### Created a simple webpage and put it under templates as index.html.j2
> we use jinja2 to insert website name and servername from varibles.\
> website_name is a variable defined in the vars/default folder main.yml file.
```html
<!doctype html>
<html>
  <head>
    <title>{{ website_name | default("Unknown Site") }}</title>
  </head>
  <body>
    <p>It worked.. <strong>{{ website_name }}</strong> on server {{ ansible_facts['nodename'] }} Anssible roles <strong>Works</strong> and is fun</p>
  </body>
</html>
```
> We also insert the server name from the ansible_facts['nodename']
#### we also split up our tasks in four files to make it easier to read.
> main.yml i always called, it calls the other ones in order.
```sh
websiteX/tasks/
├── main.yml #always runs at start
├── install.yml #install apache2
├── configure.yml #configure apache2
└── service.yml #start apache2
```
#### we create a new handler in main.yml in folder handlers
> handlers will only run once, good for restarting services after changes.\
> It's the name of the handler that will be used for identifier.
```sh
- name: restart_apache_service
  service:
    name: apache2
    state: restarted
```
> Roles should we in their own GitHub or git repository. as it can be reused then.
