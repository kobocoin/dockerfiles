## Docker build your own Kobocoin v2.1.0.3 fullnode 
```
$ git clone https://github.com/kobocoin/kobocoin-dockers
$ cd kobocoin-dockers/Source-v2103
$ docker build -t kobocoin-v2103 . 
$ mkdir /home/kobocoin/.Kobocoin
$ chmod -R 775 /home/kobocoin/.Kobocoin
$ docker run -it --rm -p 5900 --mount type=bind,source=/home/kobocoin/.Kobocoin,destination=/root/.Kobocoin kobocoin-v2103
```
Note: The home directory can be a name of your choice but make sure the `docker run` command is also updated.

The prior step creates and runs a container and gives you a command prompt on it. From that prompt:

$ . /entrypoint.sh
$ kobocoin-qt &
$ x11vnc -display :1 -usepw

entrypoint.sh sets a machine id and then sets up Xvfp so that kobocoin-qt can use it. Next, execute kobocoin-qt in demonic mode (see the trailing &) Finally, execute x11vnc so that we can see kobooin-qt from a VNC viewer on the host. When this first runs, it will ask for a password. You'll need this password in your VNC viewer.

Next,

$ docker ps
This will give you a display of all your running containers. Hopefully you'll see kobocoin-v2103 and can determine which port on the local host it's using.

Finally, run your VNC viewer of choice and connect to that port on localhost. For example, if you see that port 5900 in the container has been mapped to port 32768 on the host, you would connect to 127.0.0.1:32768. Recall that you'll need the password you set for x11vnc earlier.
