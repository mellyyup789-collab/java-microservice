# java-microservice

Starting a microservice system requires service discovery, routing, authentication, logging, and deployment setup before application work can begin. **java-microservice provides a Java and Spring-based starting point for those tasks.**

## Features

- Register services with Spring Cloud Eureka for service discovery.
- Route requests through an NGINX gateway and scale gateway containers with Docker Compose.
- Collect application logs with Elasticsearch, Logstash, and Kibana.
- Authenticate API requests with OAuth2 and JWT.
- Publish distributed events through Kafka and organize read and write flows with CQRS and event sourcing.
- Monitor applications with JMX, Spring Boot Admin, and the Turbine stream.

## Installation

Install Java 8, Docker, and Docker Compose. Then clone the repository and build its modules:

```bash
git clone https://github.com/apssouza22/java-microservice.git
cd java-microservice
./package-projects.sh
```

Start the local service environment:

```bash
./docker-orchestrate.sh
docker-compose -p todo up
```

ELK requires at least 3 GB of memory allocated to Docker. Each service may contain additional setup instructions in its module directory.

## Usage

Start the stack with the `todo` Compose project:

```bash
docker-compose -p todo up
```

Request an OAuth2 access token with the sample client credentials:

```bash
curl -X POST -vu todo-app:123456 http://localhost:8017/oauth/token -H "Accept: application/json" -d "password=1234&username=apssouza22@gmail.com&grant_type=password&scope=write&client_secret=123456&client_id=todo-app"
```

Use the returned token to call the accounts service:

```bash
curl -H "Authorization: Bearer $TOKEN" "http://localhost:8018/accounts"
```

The local services use these endpoints:

- To-dos: http://localhost:8015/todos
- Users: http://localhost:8016/accounts
- Eureka server: http://localhost:8010/
- Config server: http://localhost:8888/
- Spring Boot Admin: http://localhost:8026
- Kibana: http://localhost:5601
- Elasticsearch: http://localhost:9200
- NGINX status: http://localhost:8055/nginx_status
- Monitoring stream: http://localhost:8022/turbine.stream

Scale the gateway containers with Docker Compose after configuring the gateway ports in `default.conf`:

```bash
docker-compose -f proxy-docker-compose.yml -p todo up --scale gateway=2
```

Stop the environment with:

```bash
docker-compose -p todo down
```

## License

This project is licensed under the MIT License. It is based on [apssouza22/java-microservice](https://github.com/apssouza22/java-microservice), which is also distributed under the MIT License.
