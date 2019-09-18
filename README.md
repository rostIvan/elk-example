# elk-example
Simple example of gathering and visualization logs from server

Based on [elk](https://hub.docker.com/r/sebp/elk/) and [filebeat](https://www.docker.elastic.co/#) images

#### Flow
`Filebeat -> Logstash -> Elasticsearch  -> Kibana`

#### ELK default ports
```ruby
{
    Elasticsearch => 9200,
    Logstash => 5044,
    Kibana => 5601
}
```

#### Up Elasticsearch, Logstash, Kibana
```shell script
docker-compose up -d elk
```
if you've got an error `max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]` 

probably need to run the command below and try again
```shell script
sudo sysctl -w vm.max_map_count=262144
```

#### Configure Filebeat
Change [host](https://github.com/rostIvan/elk-example/blob/cf0bea17a0e71d7f42f8f5c65b9b73190af2d747/filebeat/conf.d/filebeat.yml#L7) to your own, for e.g.
```yaml
output.logstash:
  hosts: ["my-elk-host:5044"]
```

Set correct [volume](https://github.com/rostIvan/elk-example/blob/cf0bea17a0e71d7f42f8f5c65b9b73190af2d747/docker-compose.yml#L18) for filebeat output, for e.g.
```yaml
 volumes:
      - path/to/log/directory:/var/log/filebeat-source
```
```yaml
filebeat.inputs:
- type: log
  paths:
    - /var/log/filebeat-source/*.log
```

#### Up Filebeat
```shell script
docker-compose up -d filebeat
```