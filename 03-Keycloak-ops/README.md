# Keycloak DevOPS
## วัตถุประสงค์
สำหรับใช้ทดสอบ โดยการสร้าง OpenLDAP และ Keycloak พร้อมทั้ง sample user ที่อยู่ใน OpenLDAP ในการจำลองการล็อกอินต่างๆ

สามารถตรวจสอบ user/pass ที่ใช้ในการทดสอบได้ในไฟล์ 
- /ldap_file/newusers-large.ldif
- /ldap_file/newusers.ldif

และสามารถแก้ไขข้อมูลต่างๆได้ในไฟล์ดังกล่าว โดยไฟล์เล่านี้จะถูก import ในขั้นตอน Container startup 


### Example of a list of users in OpenLDAP.
- jsmith1/secret
- sbrown20/secret
- dev1/secret
- tester1/secret
- e9d7333c-3bd6-49b1-891f-ec1965e59c34/iHU4fUktMX
- 5fa3b5c3-06f1-439e-88e0-e6b7d40e312d/bLELPpWXb
- dbe92640-5058-40be-ad13-aae2747efba2/cpd1pZMlhzSY


# Quick Start Docker-Compose
Run OpenLDAP docker image:
```
docker-compose up -d 
```

ตรวจสอบ
```
docker-compose ps
```

เข้าหน้าเว็บ phpldapadmin สำหรับดูข้อมูลผู้ใช้ที่อยู่ใน OpenLDAP
http://localhost:7000/

> Port 7000 ตามที่ตั้งใน docker-compose.yml

Login ด้วย admin user ของ OpenLDAP
Username: cn=admin,dc=company,dc=org
Password: pass1234 

> Admin Password จะอยู่ที่ .env "LDAP_ADMIN_PASSWORD"



# Note 
## Importing A Realm On Startup
The published Keycloak containers have a directory /opt/keycloak/data/import. If you put one or more import files in that directory via a volume mount or other means and add the startup argument --import-realm, the Keycloak container will import that data on startup! This may only make sense to do in Dev mode.

Example
```
docker run --name keycloak_unoptimized -p 8080:8080 \
        -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=change_me \
        -v /path/to/realm/data:/opt/keycloak/data/import
        quay.io/keycloak/keycloak:latest \
        start-dev --import-realm
```

> Note: https://www.keycloak.org/server/containers



## Sycn User to KeyCloak
Go to "User federation > <Your-Ldap> 
And then click action "sync all user"


## Test Get OAuth2 configuration details
http://localhost:8080/realms/internal-apps/.well-known/openid-configuration


curl -k  http://localhost:8080/realms/internal-apps/protocol/openid-connect/token -d "grant_type=client_credentials" -d "client_id=super-app" -d "client_secret=Z1yWpY5xWSIsBTMg4D4ojcGP95NblNnz"
