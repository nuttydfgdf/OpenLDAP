
# Quick Start
Run OpenLDAP docker image:
```
docker-compose up -d
```

Either command starts a new container with OpenLDAP running inside. Let's make the first search in our LDAP container:
```
docker exec <my-openldap-container> ldapsearch -x -H ldap://localhost -b dc=company,dc=org -D "cn=admin,dc=company,dc=org" -w pass1234
```
OR
```
docker-compose exec openldap bash -c "ldapsearch -x -H ldap://localhost -b dc=company,dc=org -D "cn=admin,dc=company,dc=org" -w pass1234" 
```
---
**NOTE**

We can jump into the container with this command. 'docker-compose exec <services-name-in-docker-compose> bash'
<br> Example: docker-compose exec openldap bash
    
---

Output (Don't copy)
```
$ docker-compose exec openldap bash -c "ldapsearch -x -H ldap://localhost -b dc=company,dc=org -D "cn=admin,dc=company,dc=org" -w pass1234" 
# extended LDIF
#
# LDAPv3
# base <dc=company,dc=org> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# mycompany.com
dn: dc=company,dc=org
objectClass: top
objectClass: dcObject
objectClass: organization
o: MyCompany Inc.
dc: example

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

# Beginner Guide
## Using editor to manage your data in LDAP
http://localhost:7000/

Click login 

Login DN: cn=admin,dc=company,dc=org
Pass: pass1234

## Create a LDIF file for new group

For instance, if we stored our entries from the simple entry section in a file called newgroups.ldif, the command we would need to process the file and add the new entries would look something like this:

```
ldapadd -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f ./ldap_file/newgroups.ldif
ldapadd -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f ./ldap_file/newusers.ldif
 
docker-compose exec openldap bash -c "ldapadd -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f /opt/newgroups.ldif" 
docker-compose exec openldap bash -c "ldapmodify -a -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f /opt/newgroups.ldif" 

docker-compose exec openldap bash -c "ldapadd -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f /opt/newusers.ldif" 
docker-compose exec openldap bash -c "ldapmodify -a -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f /opt/newusers.ldif" 
```
You could also use the ldapmodify -a combination for the same result:
```
ldapmodify -a -x -D "cn=admin,dc=company,dc=org" -w password -H ldap:// -f newgroups.ldif
```

If you want to test it out:
```
ldapsearch -H <ldap-server-url> -x -D "<user-dn>" -w "<user-password>" -b "<user-dn>"
```
```
docker-compose exec openldap bash
ldapsearch -H ldap://localhost:389 -x -D "uid=jsmith1,ou=People,dc=company,dc=org" -w "pass1234" -b "uid=jsmith1,ou=People,dc=company,dc=org"
```
---
**NOTE**

    # Syntax to Copy from Container to Docker Host  
    docker cp {options} CONTAINER:SRC_PATH DEST_PATH 
    # Syntax to Copy from Docker Host to Container  
    docker cp {options} SRC_PATH CONTAINER:DEST_PATH 
---


## Create a LDIF file for New User
The following is a sample LDIF file that will be used to create a new user.
ech