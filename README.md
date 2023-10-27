<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&text=DockerCheatSheet&height=100&section=header"/>
</p>

Because time is money, here's the easiest docker cheatsheet you will find on the internet, period.

## Install Docker on Ubuntu:
c/p in order

#1
```
sudo apt update
```
#2
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
#3
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
#4
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
#5
```
sudo apt update
```
#6
```
apt-cache policy docker-ce
```
#7
```
sudo apt install docker-ce
```
#8
```
sudo systemctl status docker
```
##

## ENABLE Docker  API

1. Open docker.service file:
```
vi /lib/systemd/system/docker.service
```

2. Find the line starting with ExecStart and replace with this:
```
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:4243 --containerd=/run/containerd/containerd.sock
```
	  Save the Modified File
	
3. Reload the docker daemon using the below command
```
systemctl daemon-reload
```

4. Restart the docker service using the below command
```
sudo service docker restart
```

5. Confirm docker is successfully restarted
```
sudo systemctl status docker
```

You should see the following terminal output:

```
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-10-27 19:34:11 CEST; 2s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 27335 (dockerd)
      Tasks: 18
     Memory: 24.9M
        CPU: 553ms
     CGroup: /system.slice/docker.service
             └─27335 /usr/bin/dockerd -H fd:// -H=tcp://0.0.0.0:4243 --containerd=/run/containerd/containerd.sock
```

6. Test if it is working by using this command, if everything is fine below command should return an empty array.
```
curl http://localhost:4243/images/json
```

To test a remote host, replace "localhost" with the public IP address of Docker Host.

Make sure to block port 4243 from unauthorized access.

## COMMANDS CHEATSHEET


TO SEND ANY API COMMAND TO A REMOTE DOCKER HOST JUST ADD THE HOST PARAMETER AFTER "docker" BEFORE THE ACTION:

```
docker -H "ipaddressofhost:4243" build . -t nameyourimage
```

BUILD DOCKERFILE IMAGE AND DEPLOY CONTAINER :

Navigate to the folder containing your dockerfile and execute.
```
cd folderxyz
docker build . -t nameyourimage
docker run --name nameyourcontainer -p 8084:8084 -d nameofimage
```

CREATE DOCKER NETWORK (BRIDGE MODE):

```
docker network create -d bridge nameofnetwork --subnet=172.75.0.0/16
```

IF YOU WANT TO DEPLOY A CONTAINER  BUT ATTACH TO A DIFFERENT NETWORK:

```
docker run --net nameofnetwork --name nameyourcontainer -p 8084:8084 -d nameofimage
```

STOP RUNNING CONTAINER

```
docker container stop nameofcontainer
```

START RUNNING CONTAINER
```
docker container stop nameofcontainer
```

SHOW THE LAST 30 LINES OF A CONTAINER CONSOLE LOG 
```
docker logs --tail 30 nameofcontainer
```

SHOW MORE INFO ON A CONTAINER UP STATUS IN JSON FORMAT
```
docker container ls -f name=nameofcontainer --format json
```

DELETE CONTAINER
```
docker rm nameofcontainer
```

DELETE IMAGE
```
docker rmi nameofimage
```
