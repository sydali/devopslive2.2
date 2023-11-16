<<<<<<< HEAD
# devopslive2.2
ASsignment 2 Part 2



![image](https://github.com/sydali/devopslive2.2/assets/449393/7f3f7239-b848-470b-8541-9066f180d736)

```


#redis working

root@dice-devops:/home/Repos/devopslive2.2# redis-cli
127.0.0.1:6379> KEYS *
1) "hits"
127.0.0.1:6379> get "hits"
"7"
127.0.0.1:6379> exit
root@dice-devops:/home/Repos/devopslive2.2#

```

# Docker network link

```


oot@dice-devops:/home/Repos/devopslive2.2# docker inspect devopslive22_default
[
    {
        "Name": "devopslive22_default",
        "Id": "dab895b97936dffff1f79fc47163997d8a043bf48ed7079c8a6f0204aefe1b77",
        "Created": "2023-11-16T10:07:35.860815113Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.28.0.0/16",
                    "Gateway": "172.28.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "58bc4423259f1c3c0c98d85b70d5c3c57f9c6188b6b3f29dd2831dbc9237991b": {
                "Name": "devopslive22-web-1",
                "EndpointID": "48763565d06ffc85ad3a83ee35ee617ed5cb4bf56cc81e74a204a45e8c050b58",
                "MacAddress": "02:42:ac:1c:00:02",
                "IPv4Address": "172.28.0.2/16",
                "IPv6Address": ""
            },
            "8bfbc2b672c3e8de5fd153bc3e2b6c9ecce56fa7c59214a7e6e72b71310dbb5f": {
                "Name": "devopslive22-redis-1",
                "EndpointID": "a59701a815227c596307ad0a7a3baf756ea2e29ef1a3f03f9ad2002a3776a5d0",
                "MacAddress": "02:42:ac:1c:00:03",
                "IPv4Address": "172.28.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "devopslive22",
            "com.docker.compose.version": "2.3.3"
        }
    }
]

```
# New Page for App




![image](https://github.com/sydali/devopslive2.2/assets/449393/a18132db-d5d8-4279-84c2-afab5f1cdbab)


# docker scale


```
root@dice-devops:/home/Repos/devopslive2.2# docker compose up -d --remove-orphans
[+] Running 3/3
 ⠿ Container redis                    Recreated                                                                                                                                                                                                          0.3s
 ⠿ Container flask                    Running                                                                                                                                                                                                            0.0s
 ⠿ Container devopslive22-database-3  Started                                                                                                                                                                                                            0.4s
root@dice-devops:/home/Repos/devopslive2.2# docker-compose scale database=3
WARNING: The scale command is deprecated. Use the up command with the --scale flag instead.
Creating devopslive22_database_4 ... done
Creating devopslive22_database_5 ... done
root@dice-devops:/home/Repos/devopslive2.2# docker-compose ps --services
web
database
root@dice-devops:/home/Repos/devopslive2.2#  docker-compose ps
         Name                        Command               State                    Ports
-----------------------------------------------------------------------------------------------------------
devopslive22-database-3   docker-entrypoint.sh redis ...   Up      6379/tcp
devopslive22_database_4   docker-entrypoint.sh redis ...   Up      6379/tcp
devopslive22_database_5   docker-entrypoint.sh redis ...   Up      6379/tcp
flask                     flask run                        Up      0.0.0.0:8009->5000/tcp,:::8009->5000/tcp

```

# Docker monitoring with prmothis +grifana 




Live Url

http://170.64.200.116:3000/?orgId=1
loginn/pass =admin/admin

tutorial at 
https://dev.to/chinhh/server-monitoring-with-prometheus-and-grafana-266o


```
^CERRO[0049] got 3 SIGTERM/SIGINTs, forcing shutdown
root@dice-devops:/home/monitor# docker compose up -d
[+] Running 4/4
 ⠿ Container grafana        Started                                                                                                                                                                                                                      1.8s
 ⠿ Container node_exporter  Started                                                                                                                                                                                                                      1.8s
 ⠿ Container cadvisor       Started                                                                                                                                                                                                                      1.7s
 ⠿ Container prometheus     Started                                                                                                                                                                                                                      1.7s
root@dice-devops:/home/monitor# cd ..
root@dice-devops:/home# cd Repos
root@dice-devops:/home/Repos# cd devopslive2.2
root@dice-devops:/home/Repos/devopslive2.2# l
Dockerfile  README.md  __pycache__/  app.py  docker-compose.yml  redis-data/  requirements.txt  test
root@dice-devops:/home/Repos/devopslive2.2# docker compose up -d
[+] Running 3/3
 ⠿ Container flask                    Running                                                                                                                                                                                                            0.0s
 ⠿ Container devopslive22_database_5  Recreated                                                                                                                                                                                                         10.5s
 ⠿ Container devopslive22-database-5  Started                                                                                                                                                                                                            0.8s
root@dice-devops:/home/Repos/devopslive2.2#

```

# compose for grifana+ promtheus


root@dice-devops:/home/monitor# cat compose.yaml
version: '3'

volumes:
  prometheus-data:
    driver: local
  grafana-data:
    driver: local

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - /etc/prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    restart: unless-stopped
    command:
      - "--config.file=/etc/prometheus/prometheus.yml"

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana-data:/var/lib/grafana
    restart: unless-stopped

  node_exporter:
    image: quay.io/prometheus/node-exporter:latest
    container_name: node_exporter
    command:
      - '--path.rootfs=/host'
    pid: host
    restart: unless-stopped
    volumes:
      - '/:/host:ro,rslave'

  cadvisor:
    image: gcr.io/cadvisor/cadvisor
    container_name: cadvisor
    # ports:
    #   - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    devices:
      - /dev/kmsg
    restart: unless-stopped```
```


