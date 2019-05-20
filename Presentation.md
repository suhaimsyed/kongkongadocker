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

## Distributions

Kong comes in many shapes. While this repository contains its core's source
code, other repos are also under active development:

- [Kong Docker](https://github.com/Kong/docker-kong): A Dockerfile for
  running Kong in Docker.
- [Kong Packages](https://github.com/Kong/kong/releases): Pre-built packages
  for Debian, Red Hat, and OS X distributions (shipped with each release).
- [Kong Vagrant](https://github.com/Kong/kong-vagrant): A Vagrantfile for
  provisioning a development-ready environment for Kong.
- [Kong Homebrew](https://github.com/Kong/homebrew-kong): Homebrew Formula
  for Kong.
- [Kong CloudFormation](https://github.com/Kong/kong-dist-cloudformation):
  Kong in a 1-click deployment for AWS EC2
- [Kong AWS AMI](https://aws.amazon.com/marketplace/pp/B06WP4TNKL): Kong AMI on
  the AWS Marketplace.
- [Kong on Microsoft Azure](https://github.com/Kong/kong-dist-azure): Run Kong
  using Azure Resource Manager.
- [Kong on Heroku](https://github.com/heroku/heroku-kong): Deploy Kong on
  Heroku in one click.
- [Kong and Instaclustr](https://www.instaclustr.com/solutions/managed-cassandra-for-kong/): Let
  Instaclustr manage your Cassandra cluster.
- [Kubernetes Ingress Controller for Kong](https://github.com/Kong/kubernetes-ingress-controller):
  Use Kong for Kubernetes Ingress.
- [Nightly Builds][kong-nightly-master]: Builds of the master branch available
  every morning at about 9AM PST.
  
  
[kong-url]: https://konghq.com/
[kong-logo]: https://konghq.com/wp-content/uploads/2018/05/kong-logo-github-readme.png
[kong-benefits]: https://konghq.com/wp-content/uploads/2018/05/kong-benefits-github-readme.png
