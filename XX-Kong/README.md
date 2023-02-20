# Setup environment
## Upping docker Containers
	docker-compose up -d
	docker-compose ps

## Check for container connectivity
Since Kong and Keycloak are from different folder, When using `docker compose up` the networks will not be able to see each other. We can work around this by connect the two networks:

	docker network connect 03-keycloak-ops_keycloak-net kong-gateway
	docker exec -u root -it kong-gateway bash
	ping openldap


## Create Kong Service & Route

	curl -X POST http://localhost:8001/services/ \
	--data "name=httpbin" \
	--data "url=http://httpbin.org"

	curl -X POST http://localhost:8001/services/httpbin/routes \
	--data "name=httpbin" \
	--data "paths=/httpbin"

Test for connection

	curl http://localhost:8000/httpbin/anything

## Apply LDAP plugin

	curl -X POST http://localhost:8001/routes/httpbin/plugins \
 	--data "name=ldap-auth"  \
    --data "config.hide_credentials=true" \
    --data "config.ldap_host=openldap" \
    --data "config.ldap_port=389" \
    --data "config.base_dn=ou=People,dc=company,dc=org" \
    --data "config.attribute=uid" \
    --data "config.header_type=basic"
    
# Testing 

1. Testing plugin 401  (connect wo/ authentication leads to `Unauthorized`)  
	`curl http://localhost:8000/httpbin/anything`  

2. Testing login w/ dev1:secret and tester1:secret  
`curl -i -v -u dev1:secret http://localhost:8000/httpbin/anything`   
`curl -i -u tester1:secret http://localhost:8000/httpbin/anything`   

3. Testing login w/ uid  
`curl -i -u fb54526e-e6eb-441a-9b69-4014561e24d6:EF5eBdI8dg http://localhost:8000/httpbin/anything`

# Reference
[Kong's LDAP Authentication plugin](https://docs.konghq.com/hub/kong-inc/ldap-auth/)  
[curl man page](https://linux.die.net/man/1/curl)



