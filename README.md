For the task of creating a docker compose setup, for logging an NGINX server, I went with `filebeat` as my logging solution. I used filebeat because it is lightweight compared to fluentd, and since I needed a demonstration, it has a simple configuration. In addition to that, it has support for NGINX with pre-configured modules, which makes the setup easier. Apart from these reasons, it ties in well with the ELK stack. On the other hand, Fluentd is better for complex tasks, and computations. I didn't use Graylog, because I wanted to use Elasticsearch and Kibana. OpenSearch, is a fork of ElasticSearch, I didn't use it since more tutorials online directly used ElasticSearch, so I went with that. 

For this demo, `filebeat` worked out, but for larger applications `Fluentd` might be more suitable.

# Docker Compose file
- I created a standard NGINX instance, and created a shared volume for its logs.
- I also created a network, for the logging to be on.
- For the filebeat container, I kept it on the same network.
- I used elastic search, to store the logging data that is processed by the filebeat container, from the NGINX container.
- I disabled the security parameters, for demonstration purposes, and kept the discover type as single-node as well. (in the ElasticSearch)
- I used Kibana, to visualise the logs from the NGINX container. 

# Additional created files
## filebeat/filebeat.yml
- NGINX produces two types of logs - an access log and an error log.
- I captured both of them, by using the shared volume.
- In the elastic search output, I just added the timestamp, and a "log_type" for the metadata

## nginx/html/index.html
- Instead of keeping just a GET request, on the NGINX page, I created a static page that can send three types of requests, and get three different responses - 200 OK, 404 Error, 500 Error.
- This will let me test out the tagging feature of filebeat, which can be verified in kibana

## nginx/logs/
- This directory is the shared volume that is created, to store the logs of the NGINX container.

## nginx/nginx.conf
- This store the configuration for the NGINX container.

# Demonstation
- For the demonstration, I created a new index on Kibana, that searches for `nginx-*`.
- This allowed me to view the logs in one place, that is on the data explorer.
- The video of the demonstration is present in this repository under the name `Logging Demo with ELK and NGINX.mp4`.
