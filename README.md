# Docker ELK stack + Grafana 

Run the Elasticsearch, Logstash, Kibana, Grafana with Docker.

Based on the official images:

- [elasticsearch](https://www.docker.elastic.co/r/elasticsearch/elasticsearch) — 9.5.1
- [logstash](https://www.docker.elastic.co/r/logstash/logstash) — 9.5.1
- [kibana](https://www.docker.elastic.co/r/kibana/kibana) — 9.5.1
- [grafana](https://hub.docker.com/r/grafana/grafana/) — 13.1.0

## Setup

1. Install [Docker](https://docs.docker.com/get-docker/) and the
   [Compose plugin](https://docs.docker.com/compose/install/) (bundled with
   Docker Desktop; `docker compose`, not the old standalone `docker-compose`).

```
curl -fsSL https://get.docker.com | sh
```
```
sudo usermod -aG docker $USER
```

2. Clone this repository.
`curl https://github.com/dzirat/ELKG`

## Usage

Start the ELK stack + Grafana (detached mode):

```
$ docker compose up -d
```

Give Elasticsearch a few seconds to report healthy — Logstash, Kibana, and
Grafana all wait on it via a healthcheck before starting.

Inject logs via TCP or UDP as JSON lines (the shipped Logstash pipeline
expects `json_lines`):

```
$ echo '{"message": "hello from bash"}' | nc localhost 5000
```

- Access Kibana at <http://localhost:5601>.
- Access Grafana at <http://localhost:3000> (default login `admin` / `admin`,
  you'll be prompted to change it on first login). An Elasticsearch
  datasource pointing at `logs_*` is provisioned automatically.

By default, the stack exposes the following ports:

- `5000`: Logstash TCP + UDP input 1 (indexed into `logs_tcp-5000-index-%{+YYYY.MM.dd}` / `logs_udp-5000-index-%{+YYYY.MM.dd}`)
- `6000`: Logstash TCP input 2 (indexed into `logs_tcp-6000-index-%{+YYYY.MM.dd}`)
- `9200`: Elasticsearch HTTP
- `9300`: Elasticsearch transport
- `5601`: Kibana
- `3000`: Grafana

Stop everything (and drop the named volumes) with:

```
$ docker compose down -v
```

This package uses compatible versions across all the products:
- elasticsearch 9.5.1
- logstash 9.5.1
- kibana 9.5.1
- grafana 13.1.0
