# Docker ELK Stack

This repository contains a Docker Compose configuration for setting up an Elasticsearch and Kibana stack locally without security (not for production deployments)

## Prerequisites

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Services

This Docker Compose configuration sets up the following services:

### 1. Elasticsearch

- **Image**: `docker.elastic.co/elasticsearch/elasticsearch:8.15.0`
- **Container Name**: `elasticsearch`
- **Ports**: 
  - `9200:9200`

### 2. Kibana

- **Image**: `docker.elastic.co/kibana/kibana:8.15.0`
- **Container Name**: `kibana`
- **Depends On**: `elasticsearch`
- **Ports**: 
  - `5601:5601`

## Network

A custom network named `elastic-net` is created with the bridge driver.

## Getting Started

1. Clone this repository:

    ```sh
    git clone <repository_url>
    cd <repository_directory>
    ```

2. Set the required environment variables in your shell or in a `.env` file:

    ```sh
    export ELASTIC_PASSWORD=<your_elastic_password>
    export KIBANA_PASSWORD=<your_kibana_password>
    ```

3. Start the services using Docker Compose:

    ```sh
    docker-compose up -d
    ```

4. To run Kibana, you must first set the kibana_system password in the Elasticsearch container:

    ```sh
    curl -u elastic:$ELASTIC_PASSWORD \
      -X POST \
      http://localhost:9200/_security/user/kibana_system/_password \
      -d '{"password":"'"$KIBANA_PASSWORD"'"}' \
      -H 'Content-Type: application/json'
    ```

5. Access the services:

    - **Elasticsearch**: [http://localhost:9200](http://localhost:9200)
    - **Kibana**: [http://localhost:5601](http://localhost:5601)

## Stopping the Services

To stop the services, run:

```sh
docker-compose down
```