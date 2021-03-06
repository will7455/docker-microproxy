## About
Dockerized lightweight non-caching HTTP(S) proxy server
https://github.com/thekvs/microproxy

[![](https://images.microbadger.com/badges/version/shkrid/docker-microproxy.svg)](https://microbadger.com/images/shkrid/docker-microproxy)
[![](https://images.microbadger.com/badges/commit/shkrid/docker-microproxy.svg)](https://microbadger.com/images/shkrid/docker-microproxy)
[![](https://images.microbadger.com/badges/image/shkrid/docker-microproxy.svg)](https://microbadger.com/images/shkrid/docker-microproxy)

## Usage

- default login:pass - microproxy:microproxy
```
docker run -d --name http-proxy -p 3128:3128 shkrid/docker-microproxy
```

- Setup login:pass
```
docker run -d --name http-proxy -p 3128:3128 -e MP_USER=login -e MP_PASS=pass --restart=unless-stopped  shkrid/docker-microproxy
```

- Custom config:

1. Copy config from running container
 
 ```
docker cp http-proxy:/mp .
 ```
2. Edit configs
 ```
$ cat mp/microproxy.toml
listen="0.0.0.0:3128"
access_log="/dev/stdout"
activity_log="/dev/stderr"
allowed_connect_ports=[443, 80]
auth_file="auth.txt"
auth_type="basic"
forwarded_for_header="delete"
via_header="delete"
allowed_networks=["0.0.0.0/0"]

$ cat mp/auth.txt
microproxy:microproxy
 ```
3. Remove old container
 
 ```
$ docker rm -f http-proxy
 ```
4. Start new container with volume mount 
 
 ```
docker run -d --name http-proxy -p 3128:3128 -v $PWD/mp:/mp --restart=unless-stopped  shkrid/docker-microproxy
 ```
