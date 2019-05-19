Pre Requisite

1.Docker installation


1. Enter docker-compose up to start up kong servers
2. Kong admin url can be accessed over localhost:8001
3. Konga UI can be accessed over localhost:1337
	Access http://localhost:1337/#!/connections and add the kong admin URL -> http://host.docker.internal:8001
4. Enter the below commands provided you have the API or service in https://github.com/suhaimsyed/
bootmaven up and running
```
	Add your Service using the Admin API
	curl -i -X POST  --url http://localhost:8001/services/   --data 'name=cart-service-spbt'   --data 'url=http://host.docker.internal:8080/api/carts/7'

	For Linux machines :-
	"until host.docker.internal is working for every platform you can use my container acting as a NAT gateway without any manually setup https://github.com/qoomon/docker-host
	"
```

```
Add a Route for the Service
curl -i -X POST   --url http://localhost:8001/services/cart-service-spbt/routes   --data 'hosts[]=getcart.com'
```

```
Forward your requests through Kong
curl -i -X GET   --url http://localhost:8000/ \  --header 'Host: getcart.com'
```


```
To transform the respone run the below command
curl -X POST http://localhost:8001/services/cart-service-spbt/plugins \
    --data "name=response-transformer"  \
    --data "config.remove.headers=Date" \
    --data "config.remove.json=shippingAddress" \
    --data "config.add.headers=x-new-header:value,x-another-header:something" \
    --data "config.add.json=new-json-key:some_value, another-json-key:some_value" \
    --data "config.append.headers=x-existing-header:some_value, x-another-header:some_value"

```

```
To terminate the API enter the below command
curl -X POST http://localhost:8001/services/cart-service-spbt/plugins  --data "name=request-termination"     --data "config.status_code=403"     --data "config.message=Sorryy mate its shut down"
```


```
To limit the rate of calls to the service enter the below command
curl -X POST http://localhost:8001/services/cart-service-spbt/plugins  --data "name=rate-limiting"    --data "config.minute=2" 
```

```
For performing a request transformation we use a available POST API - from https://github.com/suhaimsyed/graphqltutorial

Create service and route first
curl -i -X POST  --url http://localhost:8001/services/   --data 'name=store-service-gql'   --data 'url=http://host.docker.internal:8081/graphql'

curl -i -X POST   --url http://localhost:8001/services/store-service-gql/routes   --data 'hosts[]=getstore.com'

curl -X POST http://localhost:8001/services/store-service-gql/plugins     --data "name=request-transformer"    --data "config.remove.body=query"

```


```
Configure the key-auth plugin
curl -i -X POST   --url http://localhost:8001/services/store-service-gql/plugins/   --data 'name=key-auth'
```
```
Create a Consumer through the RESTful API
curl -i -X POST   --url http://localhost:8001/consumers/   --data "username=testuser"
```

```
Provision key credentials for your Consumer
curl -i -X POST   --url http://localhost:8001/consumers/testuser/key-auth/   --data 'key=password'
```

```
Verify that your Consumer credentials are valid
curl -i -X GET   --url http://localhost:8000   --header "Host: getstore.com"   --header "apikey: password"
```

```
To Add basic Auth

curl -i -X POST   --url http://localhost:8001/services/store-service-gql/plugins/      --data "name=basic-auth"   --data "config.hide_credentials=true"

curl -X POST http://localhost:8001/consumers/testuser/basic-auth \
    --data "username=ironman" \
    --data "password=tony"

Verify the same with 
curl http://localhost:8000/services/store-service-gql     -H 'Authorization: Basic aXJvbm1hbjp0b255'

```

```

To add Oauth2 
curl -X POST http://localhost:8001/services/store-service-gql/plugins     --data "name=oauth2"  \
    --data "config.scopes=email,phone,address" \
    --data "config.mandatory_scope=true" \
    --data "config.enable_authorization_code=true"
    
 curl -X POST http://localhost:8001/consumers/testuser/oauth2 \
    --data "name=Test%20Application" \
    --data "client_id=myclientid" \
    --data "client_secret=mysupersecret" \
    --data "redirect_uris[]=http://some-domain/endpoint/"
```

```

curl -X POST http://localhost:8001/services/store-service-gql/plugins \
    --data "name=hmac-auth" 

curl -i -X POST http://localhost:8001/services/store-service-gql/plugins \
      -d "name=hmac-auth" \
      -d "config.enforce_headers=date, request-line" \
      -d "config.algorithms=hmac-sha1, hmac-sha256"
```
5. View the services and routes in konga UI
