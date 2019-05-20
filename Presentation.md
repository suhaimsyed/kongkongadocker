[![][kong-logo]][kong-url]

Kong is a cloud-native, fast, scalable, and distributed Microservice
Abstraction Layer *(also known as an API Gateway, API Middleware or in some
cases Service Mesh)*. Made available as an open-source project in 2015, its
core values are high performance and extensibility.


## Why Kong?

If you are building for the web, mobile, or IoT (Internet of Things) you will
likely end up needing common functionality to run your actual software. Kong
can help by acting as a gateway (or a sidecar) for microservices requests while
providing load balancing, logging, authentication, rate-limiting,
transformations, and more through plugins.

[![][kong-benefits]][kong-url]

## Features

- **Cloud-Native**: Platform agnostic, Kong can run from bare metal to
  Kubernetes.
- **Dynamic Load Balancing**: Load balance traffic across multiple upstream
  services.
- **Hash-based Load Balancing**: Load balance with consistent hashing/sticky
  sessions.
- **Circuit-Breaker**: Intelligent tracking of unhealthy upstream services.
- **Health Checks:** Active and passive monitoring of your upstream services.
- **Service Discovery**: Resolve SRV records in third-party DNS resolvers like
  Consul.
- **Serverless**: Invoke and secure AWS Lambda or OpenWhisk functions directly
  from Kong.
- **WebSockets**: Communicate to your upstream services via WebSockets.
- **OAuth2.0**: Easily add OAuth2.0 authentication to your APIs.
- **Logging**: Log requests and responses to your system over HTTP, TCP, UDP,
  or to disk.
- **Security**: ACL, Bot detection, whitelist/blacklist IPs, etc...
- **Syslog**: Logging to System log.
- **SSL**: Setup a Specific SSL Certificate for an underlying service or API.
- **Monitoring**: Live monitoring provides key load and performance server
  metrics.
- **Forward Proxy**: Make Kong connect to intermediary transparent HTTP proxies.
- **Authentications**: HMAC, JWT, Basic, and more.
- **Rate-limiting**: Block and throttle requests based on many variables.
- **Transformations**: Add, remove, or manipulate HTTP requests and responses.
- **Caching**: Cache and serve responses at the proxy layer.
- **CLI**: Control your Kong cluster from the command line.
- **REST API**: Kong can be operated with its RESTful API for maximum
  flexibility.
- **Geo-Replicated**: Configs are always up-to-date across different regions.
- **Failure Detection & Recovery**: Kong is unaffected if one of your Cassandra
  nodes goes down.
- **Clustering**: All Kong nodes auto-join the cluster keeping their config
  updated across nodes.
- **Scalability**: Distributed by nature, Kong scales horizontally by simply
  adding nodes.
- **Performance**: Kong handles load with ease by scaling and using NGINX at
  the core.
- **Plugins**: Extendable architecture for adding functionality to Kong and
  APIs.

For more info about plugins and integrations, you can check out the [Kong
Hub](https://docs.konghq.com/hub/).





## Database Support

As mentioned, Kong stores your configuration in a backend data store, either PostgreSQL or Cassandra. You configure Kong using a API. If you’re a user of the community version, you need to either interact with the RESTful API directly via HTTP requests using a tool like curl, or via a third-party GUI like Konga. The officially supported Kong admin GUI is for Kong Enterprise subscribers only.

## Plugin support

Kong is built on top of NGINX, which means Kong plugins must be written in Lua using the lua-nginx-module. Unfortunately, Kong does not have official support for writing plugins in nginScript, NGINX’s unique JavaScript runtime. It should also be noted that the Lua language doesn’t have sufficient adoption to make either the Github or Linkedin lists mentioned previously.

## Summary

Kong has a compelling story around performance and reliability because it is built on top of NGINX. That being said, it also exhibits some potentially cumbersome dependencies. Kong’s extensibility can be viewed as limited because the custom plugins must be written in Lua. Furthermore, using Kong also requires you to adopt either PostgreSQL or Cassandra, which is troubling if you have no experience with either database. Finally, Kong is configured entirely through an admin GUI rather than a configuration file, so tracking configuration in a source version control system like Git is difficult.


  
  
[kong-url]: https://konghq.com/
[kong-logo]: https://konghq.com/wp-content/uploads/2018/05/kong-logo-github-readme.png
[kong-benefits]: https://konghq.com/wp-content/uploads/2018/05/kong-benefits-github-readme.png

References :
https://www.lunchbadger.com/api-gateway-comparison-kong-enterprise-pricing-vs-express-gateway/
https://github.com/Kong/kong/edit/master/README.md
