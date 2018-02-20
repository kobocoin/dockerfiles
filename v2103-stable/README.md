## Docker build your own Kobocoin v2.1.0.3 fullnode 

+ Software installed - Kobocoind, kobocoin-qt
+ wallet.dat - location of your choice
+ Container size - 404MB
+ Installed as root - If built in a production environment please review security
#### Build it
```
git clone https://github.com/kobocoin/dockerfiles
cd dockerfiles/v2103-stable
docker build -t kobocoin-v2103 . (note the trailing ".")
mkdir /home/kobocoin/.Kobocoin
chmod -R 775 /home/kobocoin/.Kobocoin
docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix:ro --rm -p 5900 --mount type=bind,source=/home/kobocoin/.Kobocoin,destination=/root/.Kobocoin kobocoin-v2103
```
Note: The home directory can be any location of your choice but make sure that the `docker run` command is also updated.

The prior step creates and runs a container and gives you a command prompt on it. It also couples the working directories of the X server in the container and in the OS.

Decide if you will be running the GUI or daemon
##### If you are running the daemon then start it
```
cd /opt/Kobocoin/src
./Kobocoind &
```
##### If you are running the wallet (GUI) then start it
```
cd /opt/Kobocoin
./kobocoin-qt &
```
The wallet should display on your desktop (if you have an X server running on the host OS).

#### Using VNC
If you prefer to use VNC, then from the prompt:
```
. /entrypoint.sh
kobocoin-qt &
x11vnc -display :1 -usepw
```
entrypoint.sh sets a machine id and then sets up Xvfp so that kobocoin-qt can use it. Next, execute kobocoin-qt (note the trailing &) Finally, execute x11vnc so that we can see kobooin-qt from a VNC viewer on the host. When this first runs, it will ask for a password. You'll need this password in your VNC viewer.

Next
```
docker ps
```
This will give you a display of all your running containers. You'll see kobocoin-v2103 and can determine which port on the local host it's using.

Finally, run your VNC viewer of choice and connect to that port on localhost. For example, if you see that port 5900 in the container has been mapped to port 32768 on the host, you would connect to 127.0.0.1:32768. Recall that you'll need the password you set for x11vnc earlier.

#### Don't forget to create a new image using `docker commit`
[Docker Documentation - docker commit](https://docs.docker.com/engine/reference/commandline/commit/)

#### Problems syncing?
If you're having problems syncing with the network you may need to add nodes to the `Kobocoin.conf` file. An up to date node list can always be found here (check Satoshi:2.1.0.3 node list ):
[Kobocoin Block Explorer](https://chainz.cryptoid.info/kobo/#!network)
