# LXC-Web-UI
Web User Interface for LXC Containers

Please ensure to use al least lxc 1.0.4. 

Install From Source

Download source code and inside the source directory run these steps:

fab build_assets         # build assets using python-fabric
./setup.py develop       # install python package
mkdir -p /etc/lwp        # create config/var dirs and popolate it
mkdir -p /var/lwp
cp lwp.example.conf /etc/lwp/lwp.conf
cp lwp.db /var/lwp/lwp.db
service firewalld stop   
service lxc start        # if service lxc exists
./bin/lwp --debug        # run lwp wth debug support
Configuration

Copy /etc/lwp/lwp.example.conf to /etc/lwp/lwp.conf
Edit it to your needs.
Start lwp service as root:
service lwp start

Your LXC UI is now at http://localhost:5000/ and default username and password are admin/admin.

SSL configuration

You can configure nginx as reverse proxy if you want to use SSL encryption.

Authentication methods

Default authentication is against the internal sqlite database, but it's possible to configure alternative backends.

LDAP

To enable ldap auth you should set auth type to ldap inside your config file than configure all options inside ldap section. See lwp.example.conf for references.

Pyhton LDAP needs to be installed:

install python-ldap
htpasswd

To enable authentication against htpasswd file you should set auth type to htpasswd and file variable in htpasswd section to point to the htpasswd file.

This backend use the crypt function, here an example where -d force the use of crypt encryption when generating the htpasswd file:

htpasswd -d -b -c /etc/lwp/httpasswd admin admin
PAM

To enable authentication against PAM you should set auth type to pam and service variable in pam section. Python PAM module needs to be installed:

pip install pam

With default login service all valid linux users can login to lwp. Many more options are available via PAM Configuration, see PAM docs.

HTTP

This auth method is used to authenticate the users using an external http server through a POST request. To enable this method auth type to http and configure the option under http section.

Custom autenticators

If you want to use different type of authentication, create appropriate file in authenticators/ directory with specific structure (example can be viewed in stub authenticator)
