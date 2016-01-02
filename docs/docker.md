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
