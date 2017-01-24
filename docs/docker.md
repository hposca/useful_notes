Dicas interessantes para docker

## Exposing a port on a live container

From this [Stack Overflow hint](http://stackoverflow.com/questions/19897743/exposing-a-port-on-a-live-docker-container):

1. Discover the IP of the machine:

    docker inspect container_name | grep -i IPAddress

2. Create an IPTables rule (with root permissions):

    iptables -t nat -A  DOCKER -p tcp --dport 8001 -j DNAT --to-destination container-ip:8000

3. Now you can access the container contents through the hostsIP:port

## Limpando o lixo deixado para trás

From [this site](http://java.dzone.com/articles/docker-%E2%80%93-clean-after-yourself?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+javalobby%2Ffrontpage+%28Javalobby+%2F+Java+Zone%29):

Make sure that exited containers are deleted.

~~~
docker rm -v $(docker ps -a -q -f status=exited)
~~~

Remove unwanted ‘dangling’ images.

~~~
docker rmi $(docker images -f "dangling=true" -q)
~~~

Still not enough space? What is this ‘vfs’ directory?

~~~
docker run -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker:/var/lib/docker --rm martin/docker-cleanup-volumes
~~~

# Acessing the container to use debugging tools

In rails development sometimes we have to use [pry](https://github.com/pry/pry) via `binding.pry` to debug our code.

As we need to be on the same terminal that is running the process we need to start our server manually:

    docker-compose run --service-ports <container-name-defined-in-the-docker-compose-file> /bin/bash
    rails s

And then, when we hit the breakpoint (`binding.pry`) we can [use pry](https://github.com/pry/pry/wiki) normally.

# Checking environment variables on a live container

With the help of [1](http://stackoverflow.com/questions/34051747/get-environment-variable-from-docker-container) and [2](http://stackoverflow.com/questions/30342796/how-to-get-env-variable-when-doing-docker-inspect):

```
docker inspect --format '{{ range $index, $value := .Config.Env }}{{ println $value }}{{ end }}' <container_id>
```
