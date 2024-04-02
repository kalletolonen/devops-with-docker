# Docker MOOC-notes

Docker is a handy tool - here are my notes from the DevOps with Docker MOOC-notes.

## Handy shortcuts and trivia [1]

```
# Print all containers, pipe to grep and money was made
docker ps -a | grep abc
```

Docker uses the id for containers to operate on those, you don't need the whole hash to identify 1, ie. just use the 2 first chars, since docker will prompt you, if the query matches many items

```
docker ps -a |grep hello-world
4d59d68e83ad   hello-world                   "/hello"                 4 minutes ago   Exited (0) 4 minutes ago                                                                    exciting_solomon
kallet@xxx:/$ docker rm 4d
4d
```

Multiple args are also supported, so stopping many containers would be achieved with:

```Bash
docker stop 3d 4d 5d
```

## Ex 1

### Exercise 1.1: Getting started

Start 3 containers from an image that does not automatically exit (such as nginx) in detached mode.
Stop two of the containers and leave one container running.
Submit the output for docker ps -a which shows 2 stopped containers and one running.

```Bash
docker run -d nginx
6f186b9e97761f7bb47feb818ccee53a13c81f51197a12ae4f6d7c3d13955167
kallet@:/$ docker run -d nginx
4de61ada994e9bcb9bea4503bc9a43834d4d86be82b11516a37df51d03aaddce
kallet@:/$ docker run -d nginx
c0410050390ea13411d2618f3612e2bf5c6b651308fd08f9b6b717c81757cb27

kallet@:/$ docker stop c0 4d
c0
4d

kallet@:/$ docker ps -a
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS                         PORTS                                                           NAMES
c0410050390e   nginx                         "/docker-entrypoint.…"   25 seconds ago   Exited (0) 3 seconds ago                                                                       objective_shirley
4de61ada994e   nginx                         "/docker-entrypoint.…"   26 seconds ago   Exited (0) 3 seconds ago                                                                       inspiring_hopper
6f186b9e9776   nginx                         "/docker-entrypoint.…"   28 seconds ago   Up 27 seconds                  80/tcp                                                          dreamy_kare
```

### Exercise 1.2: Cleanup

Clean the Docker daemon by removing all images and containers.
Submit the output for docker ps -a and docker image ls

```Bash
kallet@:/$ docker ps -a | grep nginx
e5e436a916c9   nginx                         "/docker-entrypoint.…"   34 seconds ago   Exited (0) 26 seconds ago                                                                   magical_bartik
kallet@:/$ docker image ls | grep nginx
nginx                                             latest                           e4720093a3c1   3 weeks ago     187MB
nginx                                             alpine                           529b5644c430   4 months ago    42.6MB
kallet@:/$ docker rm e5
e5

kallet@:/$ docker image rm nginx:latest
Untagged: nginx:latest
Untagged: nginx@sha256:c26ae7472d624ba1fafd296e73cecc4f93f853088e6a9c13c0d52f6ca5865107
Deleted: sha256:e4720093a3c1381245b53a5a51b417963b3c4472d3f47fc301930a4f3b17666a
kallet@:/$ docker image rm nginx:alpine
Untagged: nginx:alpine
Untagged: nginx@sha256:a59278fd22a9d411121e190b8cec8aa57b306aa3332459197777583beb728f59

kallet@:/$ docker ps -a |grep nginx
kallet@:/$ docker image ls | grep nginx
kallet@:/$
```

I have other Docker stuff running, so that's why I resorted to grepping the output.

##

This will start an Ubuntu container that'll print out a date every second:

```Bash
docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'
```

You can see the output with:
```Bash
docker logs -f looper
```

Docker exec lets you run commands inside the container:
```Bash
kalle.tolonen$ docker exec looper ls -la
total 56
drwxr-xr-x   1 root root 4096 Mar 12 12:03 .
drwxr-xr-x   1 root root 4096 Mar 12 12:03 ..
-rwxr-xr-x   1 root root    0 Mar 12 12:03 .dockerenv
lrwxrwxrwx   1 root root    7 Feb 27 15:59 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Apr 18  2022 boot
drwxr-xr-x   5 root root  360 Mar 12 12:03 dev
drwxr-xr-x   1 root root 4096 Mar 12 12:03 etc
drwxr-xr-x   2 root root 4096 Apr 18  2022 home
lrwxrwxrwx   1 root root    7 Feb 27 15:59 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Feb 27 15:59 lib32 -> usr/lib32
lrwxrwxrwx   1 root root    9 Feb 27 15:59 lib64 -> usr/lib64
lrwxrwxrwx   1 root root   10 Feb 27 15:59 libx32 -> usr/libx32
drwxr-xr-x   2 root root 4096 Feb 27 15:59 media
drwxr-xr-x   2 root root 4096 Feb 27 15:59 mnt
drwxr-xr-x   2 root root 4096 Feb 27 15:59 opt
dr-xr-xr-x 218 root root    0 Mar 12 12:03 proc
drwx------   2 root root 4096 Feb 27 16:02 root
drwxr-xr-x   5 root root 4096 Feb 27 16:03 run
lrwxrwxrwx   1 root root    8 Feb 27 15:59 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Feb 27 15:59 srv
dr-xr-xr-x  11 root root    0 Mar 12 12:03 sys
drwxrwxrwt   2 root root 4096 Feb 27 16:02 tmp
drwxr-xr-x  14 root root 4096 Feb 27 15:59 usr
drwxr-xr-x  11 root root 4096 Feb 27 16:02 var
```

Or we can just jump right into the mix and execute commands from the inside:

```Bash
$ docker exec -it looper bash
root@c0ecaba8e9bc:/# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0   2888   992 pts/0    Ss+  12:03   0:00 sh -c while true; do date; sleep 1; done
root       859  0.6  0.0   4624  3756 pts/1    Ss   12:10   0:00 bash
root       874  0.0  0.0   2788  1004 pts/0    S+   12:10   0:00 sleep 1
root       875  0.0  0.0   7060  1580 pts/1    R+   12:10   0:00 ps aux
root@c0ecaba8e9bc:/#
```

When we create a Docker instance with some extra flags, we can detach from it with (ctrl + p, ctrl + q):
```Bash
docker run -d --rm -it --name looper-it ubuntu sh -c 'while true; do date; sleep 1; done'
```

The rm-flag states that the container would've been removed if we had used ctrl + c to exit [2].

### Exercise 1.3: Secret message

Image devopsdockeruh/simple-web-service:ubuntu will start a container that outputs logs into a file. 

Go inside the running container and use tail -f ./text.log to follow the logs. Every 10 seconds the clock will send you a "secret message".

Submit the secret message and command(s) given as your answer.

Starting the container:
```Bash
$ docker run -d -it --name ex1_3 devopsdockeruh/simple-web-service:ubuntu
e08cbbab0b45f7fc4896bb73f41e779423c12742550751bf682807565154fc43
```

Logging in and checking the printout:
```Bash
$ docker exec -it ex1_3 bash
root@e08cbbab0b45:/usr/src/app# cat /usr/src/app/text.log
2024-03-12 12:23:17 +0000 UTC
2024-03-12 12:23:19 +0000 UTC
2024-03-12 12:23:21 +0000 UTC
2024-03-12 12:23:23 +0000 UTC
2024-03-12 12:23:25 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-12 12:23:27 +0000 UTC
2024-03-12 12:23:29 +0000 UTC
2024-03-12 12:23:31 +0000 UTC
2024-03-12 12:23:33 +0000 UTC
2024-03-12 12:23:35 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-12 12:23:37 +0000 UTC
2024-03-12 12:23:39 +0000 UTC
2024-03-12 12:23:41 +0000 UTC
2024-03-12 12:23:43 +0000 UTC
2024-03-12 12:23:45 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-12 12:23:47 +0000 UTC
2024-03-12 12:23:49 +0000 UTC
2024-03-12 12:23:51 +0000 UTC
2024-03-12 12:23:53 +0000 UTC
2024-03-12 12:23:55 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-12 12:23:57 +0000 UTC
2024-03-12 12:23:59 +0000 UTC
2024-03-12 12:24:01 +0000 UTC
2024-03-12 12:24:03 +0000 UTC
2024-03-12 12:24:05 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-12 12:24:07 +0000 UTC
2024-03-12 12:24:09 +0000 UTC
2024-03-12 12:24:11 +0000 UTC
2024-03-12 12:24:13 +0000 UTC
root@e08cbbab0b45:/usr/src/app#
```

### Installing stuff in an Docker Ubuntu-container

Let's log in:
```bash
docker run -it ubuntu

apt-get update
apt-get install -y micro
```

### Exercise 1.4: Missing dependencies

Start a Ubuntu image with the process sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'

```bash
docker run -it ubuntu sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'
Input website:
helsinki.fi
Searching..
sh: 1: curl: not found
Input website:
```

No curl installed, let's install that:

```bash
docker run -it --rm --name websiter ubuntu
apt-get update
apt-get install -y curl
'ctrl + p, ctrl + q'

docker exec -it websiter bash
root@9d232e6e3d7d:/# curl
curl: try 'curl --help' or 'curl --manual' for more information
'ctrl + p, ctrl + q'
root@9d232e6e3d7d:/# read escape sequence

$ docker exec -it websiter sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'
Input website:
helsinki.fi
Searching..
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.22.1</center>
</body>
</html>
Input website:
```

### Exercise 1.5: Sizes of images

In the Exercise 1.3 we used devopsdockeruh/simple-web-service:ubuntu.

Here is the same application but instead of Ubuntu is using Alpine Linux: devopsdockeruh/simple-web-service:alpine.

Pull both images and compare the image sizes. Go inside the Alpine container and make sure the secret message functionality is the same. Alpine version doesn't have bash but it has sh, a more bare-bones shell.

```bash
$docker run -d devopsdockeruh/simple-web-service:alpine

$docker exec -it b0 sh
/usr/src/app # cat /usr/
bin/    lib/    local/  sbin/   share/  src/
/usr/src/app # cat /usr/src/app/text.log
2024-03-13 12:44:57 +0000 UTC
2024-03-13 12:44:59 +0000 UTC
2024-03-13 12:45:01 +0000 UTC
2024-03-13 12:45:03 +0000 UTC
2024-03-13 12:45:05 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-13 12:45:07 +0000 UTC
2024-03-13 12:45:09 +0000 UTC
2024-03-13 12:45:11 +0000 UTC
2024-03-13 12:45:13 +0000 UTC
2024-03-13 12:45:15 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-13 12:45:17 +0000 UTC
2024-03-13 12:45:19 +0000 UTC
2024-03-13 12:45:21 +0000 UTC
2024-03-13 12:45:23 +0000 UTC
2024-03-13 12:45:25 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-13 12:45:27 +0000 UTC
2024-03-13 12:45:29 +0000 UTC
2024-03-13 12:45:31 +0000 UTC
2024-03-13 12:45:33 +0000 UTC
2024-03-13 12:45:35 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-13 12:45:37 +0000 UTC
2024-03-13 12:45:39 +0000 UTC
2024-03-13 12:45:41 +0000 UTC
2024-03-13 12:45:43 +0000 UTC
2024-03-13 12:45:45 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-13 12:45:47 +0000 UTC
2024-03-13 12:45:49 +0000 UTC
2024-03-13 12:45:51 +0000 UTC
2024-03-13 12:45:53 +0000 UTC
2024-03-13 12:45:55 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-03-13 12:45:57 +0000 UTC
/usr/src/app #
```

The message is the same.

### Exercise 1.6: Hello Docker Hub

Run docker run -it devopsdockeruh/pull_exercise.

The command will wait for your input.

Navigate through the Docker hub to find the docs and Dockerfile that was used to create the image.

Read the Dockerfile and/or docs to learn what input will get the application to answer a "secret message".

Submit the secret message and command(s) given to get it as your answer.

The [readme](https://hub.docker.com/r/devopsdockeruh/pull_exercise)
```bash
$ docker run -it devopsdockeruh/pull_exercise
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```

### Creating images

Let's start from a simple echo.

```bash
$ echo "Hello, doge"
Hello, doge
```

Let's make it executable:

```bash
#hello.sh

#!/bin/sh

echo "Hello, Doge"
```

And run it:
```bash
$ ./hello.sh
Hello, Doge
```

Next, we'll create an image that'll run this.

```Dockerfile
# Dockerfile
# Start from the alpine image that is smaller but no fancy tools
FROM alpine:3.19

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the hello.sh file from this directory to /usr/src/app/ creating /usr/src/app/hello.sh
COPY hello.sh .

# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build.
# RUN chmod +x hello.sh

# When running docker run the command will be ./hello.sh
CMD ./hello.sh
```

Let's build it (from the same dir as the Dockerfile-file is located in).

```bash
$ docker build . -t hello-doge
[+] Building 2.4s (8/8) FINISHED
 => [internal] load .dockerignore                                                                               0.1s
 => => transferring context: 2B                                                                                 0.0s
 => [internal] load build definition from Dockerfile                                                            0.1s
 => => transferring dockerfile: 557B                                                                            0.0s
 => [internal] load metadata for docker.io/library/alpine:3.19                                                  2.0s
 => [1/3] FROM docker.io/library/alpine:3.19@sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8  0.1s
 => => resolve docker.io/library/alpine:3.19@sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8  0.0s
 => => sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b 1.64kB / 1.64kB                  0.0s
 => => sha256:6457d53fb065d6f250e1504b9bc42d5b6c65941d57532c072d929dd0628977d0 528B / 528B                      0.0s
 => => sha256:05455a08881ea9cf0e752bc48e61bbd71a34c029bb13df01e40e3e70e0d007bd 1.47kB / 1.47kB                  0.0s
 => [internal] load build context                                                                               0.1s
 => => transferring context: 64B                                                                                0.0s
 => [2/3] WORKDIR /usr/src/app                                                                                  0.0s
 => [3/3] COPY hello.sh .                                                                                       0.0s
 => exporting to image                                                                                          0.0s
 => => exporting layers                                                                                         0.0s
 => => writing image sha256:eb14c6994a2cd5e2c3c1d00626e6d21ff3492d1aaeab6d706b9cb4a62b796dba                    0.0s
 => => naming to docker.io/library/hello-doge
 ```
 
 Let's check the image out:
 
 ```Bash
 $ docker image ls |grep doge
hello-doge                                        latest                           eb14c6994a2c   43 seconds ago   7.38MB
```

And run it:

```bash
$ docker run hello-doge
Hello, Doge
```

Then we'll run the container in interactive mode:

```bash
$ docker run -it hello-doge sh
/usr/src/app #
```

And copy stuff to the container from another terminal:

```bash
$ touch additional.txt
kallet@M2140P2Q:/c/Users/kalle.tolonen/devops-with-docker/ex1$ docker cp ./additional.txt cool_brahmagupta:/usr/src/app
Successfully copied 1.54kB to cool_brahmagupta:/usr/src/app
```

Next we need to check that the file is present:

```bash
/usr/src/app # ls
additional.txt  hello.sh
/usr/src/app #
```

Diff also works with Docker:

```bash
$ docker diff cool_brahmagupta
C /usr
C /usr/src
C /usr/src/app
A /usr/src/app/additional.txt
C /root
A /root/.ash_history
```

We can create a new image, that'll have our changes:

```bash
$ docker commit cool_brahmagupta hello-doge-additional
sha256:56000be35102fa33f65c68d7f03fc715b719319e809c171b331bbfc1b2e9a79f

$ docker image ls |grep doge
hello-doge-additional                             latest                           56000be35102   33 seconds ago   7.38MB
hello-doge                                        latest                           eb14c6994a2c   23 hours ago     7.38MB
```

We can end up in a similar outcome by just modding the Dockerfile:

```dockerfile
# Start from the alpine image that is smaller but no fancy tools
FROM alpine:3.19

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the hello.sh file from this directory to /usr/src/app/ creating /usr/src/app/hello.sh
COPY hello.sh .

# Execute a command with `/bin/sh -c` prefix.
RUN touch additional.txt

# When running docker run the command will be ./hello.sh
CMD ./hello.sh
```

And to check the outcome:

```bash
$ docker build . -t hello-doge
[+] Building 1.6s (9/9) FINISHED
 => [internal] load build definition from Dockerfile                                                              0.0s
 => => transferring dockerfile: 507B                                                                              0.0s
 => [internal] load .dockerignore                                                                                 0.0s
 => => transferring context: 2B                                                                                   0.0s
 => [internal] load metadata for docker.io/library/alpine:3.19                                                    1.2s
 => [1/4] FROM docker.io/library/alpine:3.19@sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1  0.0s
 => [internal] load build context                                                                                 0.0s
 => => transferring context: 29B                                                                                  0.0s
 => CACHED [2/4] WORKDIR /usr/src/app                                                                             0.0s
 => CACHED [3/4] COPY hello.sh .                                                                                  0.0s
 => [4/4] RUN touch additional.txt                                                                                0.2s
 => exporting to image                                                                                            0.0s
 => => exporting layers                                                                                           0.0s
 => => writing image sha256:5d8fccd3d5b8430c10ac3ec3892fce1f6b1be0d5a0ce1ea9a6e87363612e6f52                      0.0s
 => => naming to docker.io/library/hello-doge                                                                     0.0s
$ docker run hello-doge
Hello, Doge
$ docker run hello-doge ls
additional.txt
hello.sh
```

### Exercise 1.7: Image for script

We can improve our previous solutions now that we know how to create and build a Dockerfile.

Let us now get back to Exercise 1.4.

Create a new file script.sh on your local machine with the following contents:

while true
do
  echo "Input website:"
  read website; echo "Searching.."
  sleep 1; curl http://$website
done

Create a Dockerfile for a new image that starts from ubuntu:22.04 and add instructions to install curl into that image. Then add instructions to copy the script file into that image and finally set it to run on container start using CMD.

After you have filled the Dockerfile, build the image with the name "curler".

The following should now work:
```
$ docker run -it curler
```

My Dockerfile:

```Dockerfile
FROM ubuntu:22.04

WORKDIR /usr/src/app

RUN apt-get update

RUN apt-get install -y curl

COPY script.sh .

CMD ./script.sh
```

That I built with:

```bash
$ docker build -t curler .
[+] Building 0.7s (10/10) FINISHED
 => [internal] load build definition from Dockerfile                                                              0.0s
 => => transferring dockerfile: 174B                                                                              0.0s
 => [internal] load .dockerignore                                                                                 0.0s
 => => transferring context: 2B                                                                                   0.0s
 => [internal] load metadata for docker.io/library/ubuntu:22.04                                                   0.6s
 => [1/5] FROM docker.io/library/ubuntu:22.04@sha256:77906da86b60585ce12215807090eb327e7386c8fafb5402369e421f44e  0.0s
 => [internal] load build context                                                                                 0.0s
 => => transferring context: 145B                                                                                 0.0s
 => CACHED [2/5] WORKDIR /usr/src/app                                                                             0.0s
 => CACHED [3/5] RUN apt-get update                                                                               0.0s
 => CACHED [4/5] RUN apt-get install -y curl                                                                      0.0s
 => [5/5] COPY script.sh .                                                                                        0.0s
 => exporting to image                                                                                            0.0s
 => => exporting layers                                                                                           0.0s
 => => writing image sha256:374092faab462d49cc4812b113b6e49e2a4ad9bf71bd446fff7a8f0767ce2d1d                      0.0s
 => => naming to docker.io/library/curler
```

And the results:

```bash
$ docker run -it curler
Input website:
helsinki.fi
Searching..
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.22.1</center>
</body>
</html>
Input website:
```

### Exercise 1.8: Two line Dockerfile

By default our devopsdockeruh/simple-web-service:alpine doesn't have a CMD. Instead, it uses ENTRYPOINT to declare which application is run.

We'll talk more about ENTRYPOINT in the next section, but you already know that the last argument in docker run can be used to give a command or an argument.

As you might've noticed it doesn't start the web service even though the name is "simple-web-service". A suitable argument is needed to start the server!

Try docker run devopsdockeruh/simple-web-service:alpine hello. The application reads the argument "hello" but will inform that hello isn't accepted.

In this exercise create a Dockerfile and use FROM and CMD to create a brand new image that automatically runs server.

Let's create the Dockerfile:
```Dockerfile
FROM devopsdockeruh/simple-web-service:alpine

CMD server
```

And build & run it:

```bash
$ docker build . -t web-server
[+] Building 0.2s (5/5) FINISHED
 => [internal] load .dockerignore                                                                                 0.0s
 => => transferring context: 2B                                                                                   0.0s
 => [internal] load build definition from Dockerfile                                                              0.0s
 => => transferring dockerfile: 94B                                                                               0.0s
 => [internal] load metadata for docker.io/devopsdockeruh/simple-web-service:alpine                               0.0s
 => [1/1] FROM docker.io/devopsdockeruh/simple-web-service:alpine                                                 0.0s
 => exporting to image                                                                                            0.0s
 => => exporting layers                                                                                           0.0s
 => => writing image sha256:978fbf315695ef5a3ec2e77ee411c4dcd9aa9b867fbc7ea3d26962545fda0585                      0.0s
 => => naming to docker.io/library/web-server
 
$ docker run web-server
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /*path                    --> server.Start.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

### Improving the process

One of the key concepts of development is the fact that you should figure out the process before you automate it. So to do just that, we should always do the process first manually to verify it and ** then ** automate it.

Let's install some stuff manually:

```bash
$ docker run -it ubuntu:22.04
Unable to find image 'ubuntu:22.04' locally
22.04: Pulling from library/ubuntu
Digest: sha256:77906da86b60585ce12215807090eb327e7386c8fafb5402369e421f44eff17e
Status: Downloaded newer image for ubuntu:22.04
root@50c6ce591866:/# apt-get udpate
E: Invalid operation udpate
root@50c6ce591866:/# apt-get update
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1569 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]
Get:9 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [1961 kB]
Get:10 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [44.6 kB]
Get:11 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [1079 kB]
Get:12 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1848 kB]
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1353 kB]
Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [1998 kB]
93% [15 Packages 345 kB/1998 kB 17%]                                                                    4303 kB/s 0seGet:16 http://archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [50.4 kB]
Get:17 http://archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [33.3 kB]
Get:18 http://archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [80.9 kB]
Fetched 30.3 MB in 7s (4482 kB/s)
Reading package lists... Done
root@50c6ce591866:/# apt-get install -y curl
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  ca-certificates libbrotli1 libcurl4 libldap-2.5-0 libldap-common libnghttp2-14 libpsl5 librtmp1 libsasl2-2
  libsasl2-modules libsasl2-modules-db libssh-4 openssl publicsuffix
Suggested packages:
  libsasl2-modules-gssapi-mit | libsasl2-modules-gssapi-heimdal libsasl2-modules-ldap libsasl2-modules-otp
  libsasl2-modules-sql
The following NEW packages will be installed:
  ca-certificates curl libbrotli1 libcurl4 libldap-2.5-0 libldap-common libnghttp2-14 libpsl5 librtmp1 libsasl2-2
  libsasl2-modules libsasl2-modules-db libssh-4 openssl publicsuffix
0 upgraded, 15 newly installed, 0 to remove and 1 not upgraded.
Need to get 2991 kB of archives.
After this operation, 7124 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 openssl amd64 3.0.2-0ubuntu1.15 [1186 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 ca-certificates all 20230311ubuntu0.22.04.1 [155 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnghttp2-14 amd64 1.43.0-1ubuntu0.1 [76.7 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy/main amd64 libpsl5 amd64 0.21.0-1.2build2 [58.4 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy/main amd64 publicsuffix all 20211207.1025-1 [129 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/main amd64 libbrotli1 amd64 1.0.9-2build6 [315 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libsasl2-modules-db amd64 2.1.27+dfsg2-3ubuntu1.2 [20.5 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libsasl2-2 amd64 2.1.27+dfsg2-3ubuntu1.2 [53.8 kB]
Get:9 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libldap-2.5-0 amd64 2.5.17+dfsg-0ubuntu0.22.04.1 [183 kB]
Get:10 http://archive.ubuntu.com/ubuntu jammy/main amd64 librtmp1 amd64 2.4+20151223.gitfa8646d.1-2build4 [58.2 kB]
Get:11 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libssh-4 amd64 0.9.6-2ubuntu0.22.04.3 [186 kB]
Get:12 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libcurl4 amd64 7.81.0-1ubuntu1.15 [289 kB]
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 curl amd64 7.81.0-1ubuntu1.15 [194 kB]
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libldap-common all 2.5.17+dfsg-0ubuntu0.22.04.1 [15.8 kB]
Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libsasl2-modules amd64 2.1.27+dfsg2-3ubuntu1.2 [68.8 kB]
Fetched 2991 kB in 2s (1524 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package openssl.
(Reading database ... 4393 files and directories currently installed.)
Preparing to unpack .../00-openssl_3.0.2-0ubuntu1.15_amd64.deb ...
Unpacking openssl (3.0.2-0ubuntu1.15) ...
Selecting previously unselected package ca-certificates.
Preparing to unpack .../01-ca-certificates_20230311ubuntu0.22.04.1_all.deb ...
Unpacking ca-certificates (20230311ubuntu0.22.04.1) ...
Selecting previously unselected package libnghttp2-14:amd64.
Preparing to unpack .../02-libnghttp2-14_1.43.0-1ubuntu0.1_amd64.deb ...
Unpacking libnghttp2-14:amd64 (1.43.0-1ubuntu0.1) ...
Selecting previously unselected package libpsl5:amd64.
Preparing to unpack .../03-libpsl5_0.21.0-1.2build2_amd64.deb ...
Unpacking libpsl5:amd64 (0.21.0-1.2build2) ...
Selecting previously unselected package publicsuffix.
Preparing to unpack .../04-publicsuffix_20211207.1025-1_all.deb ...
Unpacking publicsuffix (20211207.1025-1) ...
Selecting previously unselected package libbrotli1:amd64.
Preparing to unpack .../05-libbrotli1_1.0.9-2build6_amd64.deb ...
Unpacking libbrotli1:amd64 (1.0.9-2build6) ...
Selecting previously unselected package libsasl2-modules-db:amd64.
Preparing to unpack .../06-libsasl2-modules-db_2.1.27+dfsg2-3ubuntu1.2_amd64.deb ...
Unpacking libsasl2-modules-db:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Selecting previously unselected package libsasl2-2:amd64.
Preparing to unpack .../07-libsasl2-2_2.1.27+dfsg2-3ubuntu1.2_amd64.deb ...
Unpacking libsasl2-2:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Selecting previously unselected package libldap-2.5-0:amd64.
Preparing to unpack .../08-libldap-2.5-0_2.5.17+dfsg-0ubuntu0.22.04.1_amd64.deb ...
Unpacking libldap-2.5-0:amd64 (2.5.17+dfsg-0ubuntu0.22.04.1) ...
Selecting previously unselected package librtmp1:amd64.
Preparing to unpack .../09-librtmp1_2.4+20151223.gitfa8646d.1-2build4_amd64.deb ...
Unpacking librtmp1:amd64 (2.4+20151223.gitfa8646d.1-2build4) ...
Selecting previously unselected package libssh-4:amd64.
Preparing to unpack .../10-libssh-4_0.9.6-2ubuntu0.22.04.3_amd64.deb ...
Unpacking libssh-4:amd64 (0.9.6-2ubuntu0.22.04.3) ...
Selecting previously unselected package libcurl4:amd64.
Preparing to unpack .../11-libcurl4_7.81.0-1ubuntu1.15_amd64.deb ...
Unpacking libcurl4:amd64 (7.81.0-1ubuntu1.15) ...
Selecting previously unselected package curl.
Preparing to unpack .../12-curl_7.81.0-1ubuntu1.15_amd64.deb ...
Unpacking curl (7.81.0-1ubuntu1.15) ...
Selecting previously unselected package libldap-common.
Preparing to unpack .../13-libldap-common_2.5.17+dfsg-0ubuntu0.22.04.1_all.deb ...
Unpacking libldap-common (2.5.17+dfsg-0ubuntu0.22.04.1) ...
Selecting previously unselected package libsasl2-modules:amd64.
Preparing to unpack .../14-libsasl2-modules_2.1.27+dfsg2-3ubuntu1.2_amd64.deb ...
Unpacking libsasl2-modules:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Setting up libpsl5:amd64 (0.21.0-1.2build2) ...
Setting up libbrotli1:amd64 (1.0.9-2build6) ...
Setting up libsasl2-modules:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Setting up libnghttp2-14:amd64 (1.43.0-1ubuntu0.1) ...
Setting up libldap-common (2.5.17+dfsg-0ubuntu0.22.04.1) ...
Setting up libsasl2-modules-db:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Setting up librtmp1:amd64 (2.4+20151223.gitfa8646d.1-2build4) ...
Setting up libsasl2-2:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Setting up libssh-4:amd64 (0.9.6-2ubuntu0.22.04.3) ...
Setting up openssl (3.0.2-0ubuntu1.15) ...
Setting up publicsuffix (20211207.1025-1) ...
Setting up libldap-2.5-0:amd64 (2.5.17+dfsg-0ubuntu0.22.04.1) ...
Setting up ca-certificates (20230311ubuntu0.22.04.1) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.34.0 /usr/local/share/perl/5.34.0 /usr/lib/x86_64-linux-gnu/perl5/5.34 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.34 /usr/share/perl/5.34 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Updating certificates in /etc/ssl/certs...
137 added, 0 removed; done.
Setting up libcurl4:amd64 (7.81.0-1ubuntu1.15) ...
Setting up curl (7.81.0-1ubuntu1.15) ...
Processing triggers for libc-bin (2.35-0ubuntu3.6) ...
Processing triggers for ca-certificates (20230311ubuntu0.22.04.1) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.

# Now we can curl a repository

root@50c6ce591866:/# curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dl
p
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 2824k  100 2824k    0     0  1232k      0  0:00:02  0:00:02 --:--:-- 2295k

```

We need to set some priviliges to run the binary we just downloaded.

```bash
chmod a+rx /usr/local/bin/yt-dlp

root@50c6ce591866:/# ls -al /usr/local/bin/yt-dlp
-rwxr-xr-x 1 root root 2892743 Mar 18 05:31 /usr/local/bin/yt-dlp
  
```

When we try to run the binary, we see that we're missing Python:

```bash
root@50c6ce591866:/# yt-dlp
/usr/bin/env: 'python3': No such file or directory

root@50c6ce591866:/# apt-get install -y python3
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libexpat1 libmpdec3 libpython3-stdlib libpython3.10-minimal libpython3.10-stdlib libreadline8 libsqlite3-0
  media-types python3-minimal python3.10 python3.10-minimal readline-common
Suggested packages:
  python3-doc python3-tk python3-venv python3.10-venv python3.10-doc binutils binfmt-support readline-doc
The following NEW packages will be installed:
  libexpat1 libmpdec3 libpython3-stdlib libpython3.10-minimal libpython3.10-stdlib libreadline8 libsqlite3-0
  media-types python3 python3-minimal python3.10 python3.10-minimal readline-common
0 upgraded, 13 newly installed, 0 to remove and 1 not upgraded.
Need to get 6516 kB of archives.
After this operation, 23.5 MB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpython3.10-minimal amd64 3.10.12-1~22.04.3 [812 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libexpat1 amd64 2.4.7-1ubuntu0.3 [91.0 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3.10-minimal amd64 3.10.12-1~22.04.3 [2242 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3-minimal amd64 3.10.6-1~22.04 [24.3 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy/main amd64 media-types all 7.0.0 [25.5 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/main amd64 libmpdec3 amd64 2.5.1-2build2 [86.8 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy/main amd64 readline-common all 8.1.2-1 [53.5 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy/main amd64 libreadline8 amd64 8.1.2-1 [153 kB]
Get:9 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libsqlite3-0 amd64 3.37.2-2ubuntu0.3 [641 kB]
Get:10 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpython3.10-stdlib amd64 3.10.12-1~22.04.3 [1848 kB]
Get:11 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3.10 amd64 3.10.12-1~22.04.3 [508 kB]
Get:12 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpython3-stdlib amd64 3.10.6-1~22.04 [6910 B]
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3 amd64 3.10.6-1~22.04 [22.8 kB]
Fetched 6516 kB in 2s (3239 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libpython3.10-minimal:amd64.
(Reading database ... 4979 files and directories currently installed.)
Preparing to unpack .../libpython3.10-minimal_3.10.12-1~22.04.3_amd64.deb ...
Unpacking libpython3.10-minimal:amd64 (3.10.12-1~22.04.3) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../libexpat1_2.4.7-1ubuntu0.3_amd64.deb ...
Unpacking libexpat1:amd64 (2.4.7-1ubuntu0.3) ...
Selecting previously unselected package python3.10-minimal.
Preparing to unpack .../python3.10-minimal_3.10.12-1~22.04.3_amd64.deb ...
Unpacking python3.10-minimal (3.10.12-1~22.04.3) ...
Setting up libpython3.10-minimal:amd64 (3.10.12-1~22.04.3) ...
Setting up libexpat1:amd64 (2.4.7-1ubuntu0.3) ...
Setting up python3.10-minimal (3.10.12-1~22.04.3) ...
Selecting previously unselected package python3-minimal.
(Reading database ... 5283 files and directories currently installed.)
Preparing to unpack .../0-python3-minimal_3.10.6-1~22.04_amd64.deb ...
Unpacking python3-minimal (3.10.6-1~22.04) ...
Selecting previously unselected package media-types.
Preparing to unpack .../1-media-types_7.0.0_all.deb ...
Unpacking media-types (7.0.0) ...
Selecting previously unselected package libmpdec3:amd64.
Preparing to unpack .../2-libmpdec3_2.5.1-2build2_amd64.deb ...
Unpacking libmpdec3:amd64 (2.5.1-2build2) ...
Selecting previously unselected package readline-common.
Preparing to unpack .../3-readline-common_8.1.2-1_all.deb ...
Unpacking readline-common (8.1.2-1) ...
Selecting previously unselected package libreadline8:amd64.
Preparing to unpack .../4-libreadline8_8.1.2-1_amd64.deb ...
Unpacking libreadline8:amd64 (8.1.2-1) ...
Selecting previously unselected package libsqlite3-0:amd64.
Preparing to unpack .../5-libsqlite3-0_3.37.2-2ubuntu0.3_amd64.deb ...
Unpacking libsqlite3-0:amd64 (3.37.2-2ubuntu0.3) ...
Selecting previously unselected package libpython3.10-stdlib:amd64.
Preparing to unpack .../6-libpython3.10-stdlib_3.10.12-1~22.04.3_amd64.deb ...
Unpacking libpython3.10-stdlib:amd64 (3.10.12-1~22.04.3) ...
Selecting previously unselected package python3.10.
Preparing to unpack .../7-python3.10_3.10.12-1~22.04.3_amd64.deb ...
Unpacking python3.10 (3.10.12-1~22.04.3) ...
Selecting previously unselected package libpython3-stdlib:amd64.
Preparing to unpack .../8-libpython3-stdlib_3.10.6-1~22.04_amd64.deb ...
Unpacking libpython3-stdlib:amd64 (3.10.6-1~22.04) ...
Setting up python3-minimal (3.10.6-1~22.04) ...
Selecting previously unselected package python3.
(Reading database ... 5713 files and directories currently installed.)
Preparing to unpack .../python3_3.10.6-1~22.04_amd64.deb ...
Unpacking python3 (3.10.6-1~22.04) ...
Setting up media-types (7.0.0) ...
Setting up libsqlite3-0:amd64 (3.37.2-2ubuntu0.3) ...
Setting up libmpdec3:amd64 (2.5.1-2build2) ...
Setting up readline-common (8.1.2-1) ...
Setting up libreadline8:amd64 (8.1.2-1) ...
Setting up libpython3.10-stdlib:amd64 (3.10.12-1~22.04.3) ...
Setting up libpython3-stdlib:amd64 (3.10.6-1~22.04) ...
Setting up python3.10 (3.10.12-1~22.04.3) ...
Setting up python3 (3.10.6-1~22.04) ...
running python rtupdate hooks for python3.10...
running python post-rtupdate hooks for python3.10...
Processing triggers for libc-bin (2.35-0ubuntu3.6) ...
```

And so it just works!

```bash
root@50c6ce591866:/# yt-dlp

Usage: yt-dlp [OPTIONS] URL [URL...]

yt-dlp: error: You must provide at least one URL.
Type yt-dlp --help to see a list of all options.
```

Next, lets make our container with the stuff we've done above, and add a step that'll allow us to pass an argument for the yt-dlp executable:

```Dockerfile
FROM ubuntu:22.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl python3
RUN curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
RUN chmod a+x /usr/local/bin/yt-dlp

# This will allow for usage of an argument, since it provides the exucutable that is run when the container is started (and we can give arguments for it)
ENTRYPOINT ["/usr/local/bin/yt-dlp"]
```

Let's try a build & run for our new tool:
```bash
youtube-dl-container$ docker build -t yt-dlp .
[+] Building 24.7s (9/9) FINISHED
 => [internal] load build definition from Dockerfile                                                            0.0s
 => => transferring dockerfile: 466B                                                                            0.0s
 => [internal] load .dockerignore                                                                               0.0s
 => => transferring context: 2B                                                                                 0.0s
 => [internal] load metadata for docker.io/library/ubuntu:22.04                                                 0.0s
 => CACHED [1/5] FROM docker.io/library/ubuntu:22.04                                                            0.0s
 => [2/5] WORKDIR /mydir                                                                                        0.0s
 => [3/5] RUN apt-get update && apt-get install -y curl python3                                                20.1s
 => [4/5] RUN curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dl  3.5s
 => [5/5] RUN chmod a+x /usr/local/bin/yt-dlp                                                                   0.4s
 => exporting to image                                                                                          0.5s
 => => exporting layers                                                                                         0.5s
 => => writing image sha256:d4509372e9c13ec4174a9a49119fca1eec2df0eaac12f13c341d67e6e9264356                    0.0s
 => => naming to docker.io/library/yt-dlp                                                                       0.0s
$ docker run yt-dlp https://www.youtube.com/watch?v=RqzGzwTY-6w
[youtube] Extracting URL: https://www.youtube.com/watch?v=RqzGzwTY-6w
[youtube] RqzGzwTY-6w: Downloading webpage
[youtube] RqzGzwTY-6w: Downloading ios player API JSON
[youtube] RqzGzwTY-6w: Downloading android player API JSON
[youtube] RqzGzwTY-6w: Downloading m3u8 information
[info] RqzGzwTY-6w: Downloading 1 format(s): 22
[download] Destination: Smoothed Brown Noise 8-Hours - Remastered, for Relaxation, Sleep, Studying and Tinnitus ☯108 [RqzGzwTY-6w].mp4
[download]   0.5% of    2.18GiB at    3.41MiB/s ETA 10:52

 ```
 
 I could not phrase this better, so this is a direct quote from the (MOOC)[https://devopswithdocker.com/part-1/section-4/].
 
 ```
 ENTRYPOINT vs CMD can be confusing - in a properly set up image, such as our yt-dlp, the command represents an argument list for the entrypoint. By default, the entrypoint in Docker is set as /bin/sh -c and this is passed if no entrypoint is set. This is why giving the path to a script file as CMD works: you're giving the file as a parameter to /bin/sh -c.

If an image defines both, then the CMD is used to give default arguments to the entrypoint.
```

## Docker volumes and ports

```bash
docker run -v "$(pwd):/mydir" yt-dlp https://www.youtube.com/watch?v=DptFY_MszQs
```

This will save the downloaded file to the spesified directory, ie. current working directory in this case. 

### Exercise 1.9: Volumes

In this exercise we won't create a new Dockerfile.

Image devopsdockeruh/simple-web-service creates a timestamp every two seconds to /usr/src/app/text.log when it's not given a command. Start the container with a bind mount so that the logs are created into your filesystem.

Submit the command you used to complete the exercise.

```bash
$ docker run -d -v ${PWD}/text.log:/usr/src/app/text.log devopsdockeruh/simple-web-service:ubuntu

$ cat text.log
2024-04-02 09:09:04 +0000 UTC
2024-04-02 09:09:06 +0000 UTC
2024-04-02 09:09:08 +0000 UTC
2024-04-02 09:09:10 +0000 UTC
2024-04-02 09:09:12 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-04-02 09:09:14 +0000 UTC
```

Exposing a port in Docker lingo means that Docker will listen to said port. Publishing the port means that Docker will map host ports to container ports, ie. it'll facilitate communication to and from the container.

### Exercise 1.10: Ports open

In this exercise, we won't create a new Dockerfile.

The image devopsdockeruh/simple-web-service will start a web service in port 8080 when given the argument "server". In Exercise 1.8 you already did an image that can be used to run the web service without any argument.

Use now the -p flag to access the contents with your browser. The output to your browser should be something like: { message: "You connected to the following path: ...

Submit your used commands for this exercise.

```bash
$ docker run -d -p 9999:8080 devopsdockeruh/simple-web-servi
ce:ubuntu server
006d4aded0aaecf089b12a0b6bbf3315f9d749800f9f17670576b3445ca39249

$ curl localhost:9999
{"message":"You connected to the following path: /","path":"/"}
````





## Sources
[1](https://devopswithdocker.com/part-1/section-1/)
[2](https://devopswithdocker.com/part-1/section-2)