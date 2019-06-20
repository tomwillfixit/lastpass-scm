# lastpass-scm

Using LastPass notes to store bash scripts. Why? For the craic.

Bash scripts stored in Github? Credentials stored in LastPass?  Just for fun add the bash scripts into LastPass and run from there.

Create generic note type in LastPass, call it "test.sh" :

```
#!/bin/bash

echo "Creating Docker secret from LastPass credential"
lpass show -p "mysecrets" > .secret

docker secret create mysecret .secret

docker secret ls

rm .secret

docker service create --name redis --secret mysecret redis:alpine

echo "Exec into Container and access secret in /run/secrets/mysecret"
```

To run locally :

```

You need to be in a Docker Swarm :  docker swarm init

Run the script : lpass show --notes "test.sh" | bash

```

Example Output :

```
Creating Docker secret from LastPass credential
8fpw55e36sfx1m07heq0pvqsu
ID                          NAME                DRIVER              CREATED             UPDATED
8fpw55e36sfx1m07heq0pvqsu   mysecret                                25 minutes ago      25 minutes ago

overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
verify: Service converged

Exec into Container and access Secret in /run/secrets/mysecret

root@ubuntu-cosmic:~# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
7ea86ed35073        redis:alpine        "docker-entrypoint.s…"   12 seconds ago      Up 9 seconds        6379/tcp            redis.1.ae35joetxd2vulhz3215wlkot

root@ubuntu-cosmic:~# docker exec -it 7ea86ed35073 /bin/sh
/data # cat /run/secrets/mysecret
203kd0eoeiw038esoe239w82jw8j934823jw
/data #

```
