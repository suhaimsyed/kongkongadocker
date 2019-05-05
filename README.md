Pre Requisite

1.Docker installation


1. Enter docker-compose up to start up kong servers
2. Kong admin url can be accessed over localhost:8001
3. Konga UI can be accessed over localhost:1337
	Access http://localhost:1337/#!/connections and add the kong admin URL -> http://host.docker.internal:8001
4. Enter the below commands provided you have the API or service in https://github.com/suhaimsyed/springbootmaven up and running
```
	Add your Service using the Admin API
	curl -i -X POST  --url http://localhost:8001/services/   --data 'name=cart-service'   --data 'url=http://host.docker.internal:8080/api/carts'

	For Linux machines :-
	"until host.docker.internal is working for every platform you can use my container acting as a NAT gateway without any manually setup https://github.com/qoomon/docker-host
	"
```

```
Add a Route for the Service
curl -i -X POST   --url http://localhost:8001/services/cart-service/routes   --data 'hosts[]=getcart.com'
```

```
Forward your requests through Kong
curl -i -X GET   --url http://localhost:8000/ \  --header 'Host: getcart.com'
```

```
Configure the key-auth plugin
curl -i -X POST   --url http://localhost:8001/services/cart-service-1/plugins/   --data 'name=key-auth'
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
curl -i -X GET   --url http://localhost:8000   --header "Host: getcart.com"   --header "apikey: password"
```

5. View the services and routes in konga UI
