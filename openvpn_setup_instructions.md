# DOCKER INSTALLATION: 
2. Install Docker (on Ubuntu):

> sudo apt-get update <br>

> sudo apt-get install ca-certificates curl gnupg <br>
  

> sudo install -m 0755 -d /etc/apt/keyrings <br>
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg <br>
  sudo chmod a+r /etc/apt/keyrings/docker.gpg

> echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 

2. Install Docker Engine:

> sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin \

3. Verify Docker successful intsallation:

> sudo docker run hello-world

# OPENVPN CONTAINER STARTUP AND CONFIGURATION:

1. Pull down the image (kylemanna/openvpn from hub.docker.com):

> docker pull kylemanna/openvpn

2. Create a folder named openvpn:

> sudo md openvpn

3. Setup openVPN Server:

- Create and initialize openvpn (create openvpn config):
>docker run --rm -v $PWD:/etc/openvpn kylemanna/openvpn ovpn_genconfig -u udp://<your IP address):1194

- Create certificate:
>docker run --rm -v $PWD:/etc/openvpn -it kylemanna/openvpn ovpn_initpki

- Create client account:
>docker run --rm -v $PWD:/etc/openvpn -it kylemanna/openvpn easyrsa build-client-full client

- Copy client certificate (ovpn file) from container:
>docker run --rm -v $PWD:/etc/openvpn kylemanna/openvpn ovpn_getclient client > client.ovpn

- Start OpenVPN container:
>docker run --name openvpn -v $PWD:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN --restart always kylemanna/openvpn

- Check if container is running: 
>docker ps

- Check container logs: 
>docker logs openvpn
