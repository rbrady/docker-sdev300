How to use


Install Docker

Ubuntu
https://docs.docker.com/install/linux/docker-ce/ubuntu/

Fedora
https://docs.docker.com/install/linux/docker-ce/fedora/#install-docker-ce-1

Mac
https://docs.docker.com/docker-for-mac/install/

Windows
https://docs.docker.com/docker-for-windows/install/

Install Docker Compose
https://docs.docker.com/compose/install/
Obtain the image configuration code
```bash
$ git clone https://github.com/rbrady/docker-sdev300.git
$ cd docker-sdev300/
$ docker-compose up -d
```

Run the image
```bash
$ docker run -d dockerlamp_php
```

Verify if the image is working properly by opening a browser window and entering in the IP of the docker container.  To get the IP of a docker container, use docker inspect command:
```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
5da1be375d89        dockerlamp_php      "docker-php-entryp..."   22 minutes ago      Up 22 minutes       0.0.0.0:80->80/tcp   dockerlamp_php_1
033cce66e515        dockerlamp_mysql    "docker-entrypoint..."   22 minutes ago      Up 22 minutes       3306/tcp             dockerlamp_mysql_1

$ docker inspect 5da1be375d89 | grep "IPAddress"
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "172.18.0.3",
```
In this example, we would open up the web browser and enter 172.18.0.3.  You should see phpinfo() displayed on the resulting web page.

Adding Web Files

To add php files to the running container, you should already have a directory mounted on your local machine that maps to /var/www/html on the container.   Run 'docker inspect' again and search for the “Mounts” section.

```bash
$ docker inspect 5da1be375d89
...
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/rbrady/code/mine/docker-lamp/html",
                "Destination": "/var/www/html",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
...
```
Running PHP scripts interactively

You can run scripts in the container using the 'docker exec' command.  For example, this is how to run a bash shell in our running php container:
```bash
$ docker exec -it 5da1be375d89 /bin/bash
```
If you copy a file into the mounted directory on your machine, you should be able to reach it on the container as well.  For example:
```bash
# create the script
echo '<?php
> Echo "Hello, World!";
> ?>' > echo.php

# run the script
docker exec -it 5da1be375d89 php /var/www/html/echo.php```

