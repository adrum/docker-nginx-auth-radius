# docker-nginx-auth-radius

This repo compiles an nginx server with radius auth.

## How To Use

Create a conf folder and initialize default vhost and radius server settings. Then mount inside docker container. See docker-compose.yml

## ENV

```
RADIUS_SERVER_URL=192.168.1.1:1812
RADIUS_SERVER_SECRET=super-secret
RADIUS_SERVER_AUTH_METHOD=#One of: PAP CHAP MSCHAP MSCHAP2 EAPMD5
```

## INFO

I've used this as an auth_request module server:

https://www.0ink.net/2019/05/10/nginx_mod_authrequest.html


## Thanks

  - https://github.com/qudreams/libmyradclient
	- https://github.com/qudreams/nginx-http-radius-module
  - https://marcofranssen.nl/nginx-1-19-supports-environment-variables-and-templates-in-docker/
