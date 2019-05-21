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

base64(ironman:tony) =aXJvbm1hbjp0b255

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
    --data "client_id=SOME-CLIENT-ID" \
    --data "client_secret=SOME-CLIENT-SECRET" \
    --data "redirect_uris[]=http://some-domain/endpoint/"
   
 curl -X POST \
  --url "https://localhost:8443/user-service-pyfl/oauth2/authorize" \
  --header "Host: getuser.com" \
  --data "response_type=code" \
  --data "client_id=SOME-CLIENT-ID" \
  --data "scope=email" \
  --data "authenticated_userid=testuser" \
  --data "provision_key=a2HagWoPahLtW6uQ2xKsqCMV1avc4hZh" \
  --insecure  
  
  In response you would get a code which can be used below
  {
    "redirect_uri": "http://some-domain/endpoint/?code=fucx0wbtHIXhxrtIAf1NZDkra4nepGv8"
}
   
  curl -X POST \
  --url "https://localhost:8443/user-service-pyfl/oauth2/token" \
  --header "Host: getuser.com" \
  --data "grant_type=authorization_code" \
  --data "client_id=SOME-CLIENT-ID" \
  --data "client_secret=SOME-CLIENT-SECRET" \
  --data "redirect_uri=http://some-domain/endpoint/" \
  --data "code=Yj5z3ZH1Cc20pT9pgLoi0AbUp1QjcgWR" \
  --insecure  
  {
    "refresh_token": "VffLxdutgcySa9dMuVqwz691j3fpSv95",
    "token_type": "bearer",
    "access_token": "ZbAVsA5qzhiJo1E4qbJMzoTDsYwDCl3v",
    "expires_in": 7200
}

curl -X GET http://kongapi.ddns.net:8000/users -header "Host: getuser.com" \
"Authorization:bearer ZbAVsA5qzhiJo1E4qbJMzoTDsYwDCl3v"
    
```

```

curl -X POST http://localhost:8001/services/store-service-gql/plugins \
    --data "name=hmac-auth" 

curl -i -X POST http://localhost:8001/services/store-service-gql/plugins \
      -d "name=hmac-auth" \
      -d "config.enforce_headers=date, request-line" \
      -d "config.algorithms=hmac-sha1, hmac-sha256"


export BODY="a small body"
export DATE=$(date -u +%a,\ %d\ %b\ %Y\ %H:%M:%S\ GMT)
export HMAC_MESSAGE=$(echo "host: getuser.com" ; echo -n "date: $DATE")
export SIGNATURE=$( echo -n "$HMAC_MESSAGE" | openssl sha -binary -sha256 -hmac secret | base64)
export DIGEST=$(echo -n "$BODY" | openssl sha -binary -sha256 | base64)
echo $DATE
echo $HMAC_MESSAGE
echo $SIGNATURE
echo $DIGEST
curl -i -X GET http://localhost:8000/users \
      -H "host: getuser.com" \
      -H "date: $DATE" \
      -H "digest: SHA-256=$DIGEST" \
      -H "authorization: hmac username=\"testuser\", algorithm=\"hmac-sha256\", headers=\"host date\", signature=\"$SIGNATURE\"" \
      -d "$BODY"
```


```
To add JWT tokens
curl -X POST http://localhost:8001/consumers/testuser/jwt -H "Content-Type: application/x-www-form-urlencoded"
curl -X POST http://localhost:8001/services/user-service-pyfl/plugins     --data "name=jwt" 
curl -X GET http:/localhost/:8000/users
-header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJIQndYbWpwS3JIT0ZpUm03TkFOVDFyR2FNamc3SEQ3UyIsInVzZXJuYW1lIjoidGVzdHVzZXIifQ.8dudOfPxZP4NUU3OlW2RrYsxcWPT6phi1ZS5DQlNCL4"



```

```
5. View the services and routes in konga UI
