<<<<<<< HEAD
# devopslive2.1
ASsignment 2 Part 1

```
root@dice-devops:/home/Repos/devopslive2.1# docker compose up -d
[+] Running 1/1
 ⠿ Container nginx_container  Started                                                                                                                                                                                                                    0.6s
root@dice-devops:/home/Repos/devopslive2.1# docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
67d53fdcca74   bridge       bridge    local
7336833188a0   host         host      local
fa82bac86cea   my_network   bridge    local
4043b45d65ff   none         null      local
root@dice-devops:/home/Repos/devopslive2.1# docker compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
nginx_container     "/docker-entrypoint.…"   web                 running             0.0.0.0:8080->80/tcp, :::8080->80/tcp
root@dice-devops:/home/Repos/devopslive2.1#

```


![image](https://github.com/sydali/devopslive2.1/assets/449393/895cdfd9-d009-4fae-add3-0a098cea0100)



```
root@dice-devops:/home/Repos/devopslive2.1# docker compose up -d
[+] Running 2/2
 ⠿ Container nginx_container  Started                                                                                                                                                                                                                    0.8s
 ⠿ Container httpd_container  Started                                                                                                                                                                                                                    0.7s
root@dice-devops:/home/Repos/devopslive2.1# docker compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
httpd_container     "httpd-foreground"       web-server          running             0.0.0.0:8081->80/tcp, :::8081->80/tcp
nginx_container     "/docker-entrypoint.…"   web                 running             0.0.0.0:8080->80/tcp, :::8080->80/tcp



```

![image](https://github.com/sydali/devopslive2.1/assets/449393/8505dee9-8fbc-4820-983f-eaafafee7019)


```
root@dice-devops:/home/Repos/devopslive2.1# docker network inspect my_network
[
    {
        "Name": "my_network",
        "Id": "fa82bac86cea8f54ef54a07b2b96ea542e637fcff2d9891be8e24f61ef05a491",
        "Created": "2023-11-16T07:44:31.89003619Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.23.0.0/16",
                    "Gateway": "172.23.0.1"
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
            "29b76658ce26b2ac1fc43584b4da617788d450917b9acade40d6ee17ce234b48": {
                "Name": "httpd_container",
                "EndpointID": "602b4844dd579bf1f4039263dadf8c9b396b5dbfcf526a46fe463124cb08d798",
                "MacAddress": "02:42:ac:17:00:02",
                "IPv4Address": "172.23.0.2/16",
                "IPv6Address": ""
            },
            "4658751d65e098a05dce104e495a6052020fa230428c6a446cff6f5ec27cec86": {
                "Name": "nginx_container",
                "EndpointID": "cc28e84206ff0780a6dd56aedbe29e59cfd8eb98d4134ae71e88a83cc76868ae",
                "MacAddress": "02:42:ac:17:00:03",
                "IPv4Address": "172.23.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "frontend",
            "com.docker.compose.project": "devopslive21",
            "com.docker.compose.version": "2.3.3"
        }
    }

```

# The two containers are attaached to same network

```
oot@dice-devops:/home/Repos/devopslive2.1# docker compose up
[+] Running 1/1
 ⠿ Container nginx_container_2  Created                                                                                                                                                                                                                  0.1s
Attaching to httpd_container, nginx_container_2
nginx_container_2  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginx_container_2  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginx_container_2  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginx_container_2  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
nginx_container_2  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
nginx_container_2  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
nginx_container_2  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginx_container_2  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginx_container_2  | /docker-entrypoint.sh: Configuration complete; ready for start up
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: using the "epoll" event method
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: nginx/1.25.3
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: OS: Linux 5.15.0-67-generic
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: start worker processes
nginx_container_2  | 2023/11/16 08:40:41 [notice] 1#1: start worker process 28
httpd_container    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.23.0.3. Set the 'ServerName' directive globally to suppress this message
httpd_container    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.23.0.3. Set the 'ServerName' directive globally to suppress this message
httpd_container    | [Thu Nov 16 08:40:41.372652 2023] [mpm_event:notice] [pid 1:tid 139966350382976] AH00489: Apache/2.4.58 (Unix) configured -- resuming normal operations
httpd_container    | [Thu Nov 16 08:40:41.374784 2023] [core:notice] [pid 1:tid 139966350382976] AH00094: Command line: 'httpd -D FOREGROUND'
^CGracefully stopping... (press Ctrl+C again to force)
[+] Running 2/2
 ⠿ Container httpd_container    Stopped                                                                                                                                                                                                                  1.2s
 ⠿ Container nginx_container_2  Stopped                                                                                                                                                                                                                  0.2s
canceled
root@dice-devops:/home/Repos/devopslive2.1# docker compose up -d --remove-orphans
[+] Running 2/2
 ⠿ Container nginx_container_2  Started                                                                                                                                                                                                                  0.7s
 ⠿ Container httpd_container    Running                                                                                                                                                                                                                  0.0s
root@dice-devops:/home/Repos/devopslive2.1# docker compos ps
docker: 'compos' is not a docker command.
See 'docker --help'
root@dice-devops:/home/Repos/devopslive2.1# docker compose  ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
httpd_container     "httpd-foreground"       web-server          running             0.0.0.0:8081->80/tcp, :::8081->80/tcp
nginx_container_2   "/docker-entrypoint.…"   web2                running             0.0.0.0:8082->80/tcp, :::8082->80/tcp
root@dice-devops:/home/Repos/devopslive2.1#


```




```
root@dice-devops:/home/Repos/devopslive2.1# docker stop nginx_container
nginx_container
root@dice-devops:/home/Repos/devopslive2.1# docke rm nginx_container -f
Command 'docke' not found, did you mean:
  command 'docker' from deb docker.io (24.0.5-0ubuntu1~22.04.1)
  command 'docker' from deb podman-docker (3.4.4+ds1-1ubuntu1.22.04.2)
Try: apt install <deb name>
root@dice-devops:/home/Repos/devopslive2.1# docker  rm nginx_container -f
nginx_container
root@dice-devops:/home/Repos/devopslive2.1#

```
![image](https://github.com/sydali/devopslive2.1/assets/449393/e57420b0-3f59-49e3-9ad1-5226e948a31c)



```
#listing

root@dice-devops:/home/Repos/devopslive2.1# docker container ls
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS          PORTS                                       NAMES
ad5b2d7b0e63   nginx        "/docker-entrypoint.…"   7 minutes ago    Up 7 minutes    0.0.0.0:8082->80/tcp, :::8082->80/tcp       nginx_container_2
f76d4e9ce163   httpd        "httpd-foreground"       11 minutes ago   Up 10 minutes   0.0.0.0:8081->80/tcp, :::8081->80/tcp       httpd_container


#stopping

root@dice-devops:/home/Repos/devopslive2.1# docker compose stop
[+] Running 2/2
 ⠿ Container httpd_container    Stopped                                                                                                                                                                                                                  1.2s
 ⠿ Container nginx_container_2  Stopped                                                                                                                                                                                                                  0.2s

#removing

root@dice-devops:/home/Repos/devopslive2.1# docker compose rm -f
Going to remove nginx_container_2, httpd_container
[+] Running 2/0
 ⠿ Container httpd_container    Removed                                                                                                                                                                                                                  0.0s
 ⠿ Container nginx_container_2  Removed                                                                                                                                                                                                                  0.0s
root@dice-devops:/home/Repos/devopslive2.1#


```


=======
# devopslive2.2
>>>>>>> 3fec9b9d516f9a99bd5b4071c19c842e7516cf74
