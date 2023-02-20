docker-compose up -d
docker-compose ps

docker network connect 03-keycloak-ops_keycloak-net kong-gateway
docker exec -u root -it kong-gateway bash
ping openldap

Go to Kong Manage create httpbin route & service

//Test
curl http://localhost:8000/httpbin/anything

//Apply Plugin
curl -X POST http://localhost:8001/routes/httpbin/plugins \
  --data "name=ldap-auth"  \
    --data "config.hide_credentials=true" \
    --data "config.ldap_host=openldap" \
    --data "config.ldap_port=389" \
    --data "config.base_dn=ou=People,dc=company,dc=org" \
    --data "config.attribute=uid" \
    --data "config.header_type=basic"
    

curl http://localhost:8000/httpbin/anything
curl -i -v -u dev1:secret http://localhost:8000/httpbin/anything
curl -i -u tester1:secret http://localhost:8000/httpbin/anything
curl -i -u fb54526e-e6eb-441a-9b69-4014561e24d6:EF5eBdI8dg http://localhost:8000/httpbin/anything

**-------------


