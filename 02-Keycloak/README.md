
# Quick Start
Run OpenLDAP docker image:
```
docker-compose up -d
docker-compose exec openldap bash -c "ldapadd -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f /opt/newgroups.ldif" 
docker-compose exec openldap bash -c "ldapadd -x -D "cn=admin,dc=company,dc=org" -w pass1234 -H ldap:// -f /opt/newusers.ldif" 
```

If you want to test it out:
```
ldapsearch -H <ldap-server-url> -x -D "<user-dn>" -w "<user-password>" -b "<user-dn>"
```
```
docker-compose exec openldap bash
ldapsearch -H ldap://localhost:389 -x -D "uid=jsmith1,ou=People,dc=company,dc=org" -w "secret" -b "uid=jsmith1,ou=People,dc=company,dc=org"
```
## Import large User

docker-compose exec openldap bash
ldapmodify -x -D "cn=admin,dc=company,dc=org" -w pass1234 -a -H ldap:// -f /opt/newusers-large.ldif

# Keycloak add intergrate OpenLDAP
http://localhost:9090/
admin/admin

Create Realm 
Go to 'User federation > Settings'
Vendor: Other
Connection URL: ldap://openldap:389
Bind type: simple
Bind DN: cn=admin,dc=company,dc=org 
Bind credentials: <admin-pass>

Edit mode: read
Users DN: ou=People,dc=company,dc=org
Username LDAP attribute: uid
RDN LDAP attribute: uid
UUID LDAP attribute: entryUUID
User object classes: inetOrgPerson, organizationalPerson

For testing LDAP user login
Go to Clients menu
At record 'account-console' click link colum 'Home URL' and try to login with user in openLdap.
After logged click panel 'Personal info' 

> Note: https://documentation.abas.cloud/en/abas-keycloak/setup-user-federation-ldap.html

> Note: https://dmc.datical.com/administer/configure-keycloak-ldap.htm


## Sycn User to KeyCloak
Go to "User federation > <Your-Ldap> 
And then click action "sync all user"


## Test Get OAuth2 configuration details
http://localhost:8080/realms/internal-apps/.well-known/openid-configuration


curl -k  http://localhost:8080/realms/internal-apps/protocol/openid-connect/token -d "grant_type=client_credentials" -d "client_id=super-app" -d "client_secret=Z1yWpY5xWSIsBTMg4D4ojcGP95NblNnz"

