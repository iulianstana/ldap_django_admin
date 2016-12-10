# Introduction #

Create a small app that will hold in a docker container a ldap server.
Within the aplication there will be a django server that will connect to my
docker ldap server and use that credentials.

The `Dockerfile` & ldap schema files are copied from
https://github.com/larrycai/docker-openldap

The own sample user data `files/users.ldif` is referred to
http://www.zytrax.com/books/ldap/ch5/ (will have my users)

# Install and Start #

    $ docker pull larrycai/openldap
    $ docker run -d -p 389:389 --name ldap -t larrycai/openldap
    $ docker ps
    user@ubuntu:/mnt/git/docker-gerrit/tmp$ docker ps
    CONTAINER ID        IMAGE                      COMMAND
CREATED             STATUS              PORTS                    NAMES
    4248037c0ab6        larrycai/openldap:latest   /bin/sh -c 'slapd -h   22
seconds ago      Up 21 seconds       0.0.0.0:63389->389/tcp   ldap

## Verify the data inside the ldap database ##

Use `ldapsearch` to check the data, 

    $ docker exec -it ldap bash
    # ldapsearch -H ldap://localhost -LL -b ou=Users,dc=openstack,dc=org -x
    version: 1

    dn: ou=Users,dc=openstack,dc=org
    objectClass: organizationalUnit
    ou: Users

    dn: cn=Robert Smith,ou=Users,dc=openstack,dc=org
    objectClass: inetOrgPerson
    .....

Or use jXplorer utilitar to se data.


## Important data ##

The admin user/passwd and BaseDN list below

    LDAP username                  : cn=admin,dc=openstack,dc=org
    cn=admin,dc=openstack,dc=org's password : password
    Account BaseDN                 [DC=168,DC=56,DC=153:49154]:
ou=Users,dc=openstack,dc=org
    Group BaseDN                   [ou=Users,dc=openstack,dc=org]:

# Customize your own data #

Use file to update ldap sever with some content `files/users.ldif`

    ldapadd -x -D cn=admin,dc=openstack,dc=org -w password -c -f users.ldif

    ldapadd -v -h localhost:389 -c -x -D cn=admin,dc=openstack,dc=org -W -f
users.ldif


