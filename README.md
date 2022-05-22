# Lightweight-network-attack-and-defense-learning-simulation-environment

OS:Linux  

1. OS environment  
```bash
sudo apt update 
sudo apt install -y git vim make docker.io docker-compose python3 curl
sudo chmod 666 /var/run/docker.sock
```

2. install  
  - Clone project
```bash
git clone https://github.com/guan4tou2/Lnadlse.git
cd Lnadlse
```
  - Data-process-module install  
```bash
cd ELK
make
```
After above command,you can use `make check` to check is elasticsearch successful install and running.  
If your kibana get error,it may be server.publicBaseUrl,you can try replace url elasticsearch to your ELK host ip in kibana/config/kibana.yml. And restart it `docker restart docker-elk_kibana`.  
If you need api key,you can use `make apikey` to set it.  

  - Attack-and-Defense-module install  
```bash
cd Machines
make
```
After installed,use `make setbeat` to install packetbeat on targeter.  
It will build attack-and-defense-environment by default.Attacker is **kali(GUI)**,Targeter is **httpd** with packetbeat.  
You can change machines by make.py. Use `python3 make.py -h` to see what machines can used.  

 - aider-module install  
```bash
docker pull portainer/portainer
docker run -d -p 9000:9000 --restart=always --name portainer -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```
[Portainer](https://github.com/portainer/portainer) can help you to manage your docker container.   
It's not necessarily to install,but recommended.  

3. Useage  
  - Data-process-module  
  Use kibana with `http://localhost:5601`  
  username and password is `elastic` adn `changeme`.  
  You can change password after you installed Data-process-module,find the file named `.env`.  
  - Attack-and-Defense-module  
  Use ssh or rdp to connect attacker  
    - SSH `ssh kali@127.0.0.1 -p 222`  
    - RDP `kali@127.0.0.1 port:3389`
      - Windows **mobaxterm**
      - Linux **remmina**  
      
   password:kali@123  
   You can change username and password,find the file what image you want in Attacker folder before you install Attack-and-Defense-module.  
   In attacker,if you want more tools,`sudo apt install -y <kali-linux-default> or <kali-linux-large>`.  

4. Remove
  - Data-process-module
```bash
cd ELK
make remove
```
  - Attack-and-Defense-module 
```bash
cd Machines
make remove
```
 - aider-module
```bash
docker stop portainer
```

5. Debug
If `make setbeat` get error,you can try to change makefile in targeter.  
Use `docker exec `docker ps -aqf "name=targeter"` /bin/bash` to connect,makefile will store at home directory.  
```
#sed -i 's/hosts: \["localhost:9200"\]/hosts: \["elasticsearch:9200"\]/g' packetbeat.yml && \
sed -i 's/#username: "elastic"/username: "elastic"/g' packetbeat.yml && \
sed -i 's/#password: "changeme"/password: "changeme"/g' packetbeat.yml && \
sed -i 's/#host: "localhost:5601"/host: "localhost:5601"\n  username: "elastic"\n  password: "changeme"/g' packetbeat.yml 
```
change hosts url to your ip.  
```
sed -i 's/hosts: \["localhost:9200"\]/hosts: \["elasticsearch:9200"\]/g' packetbeat.yml && \
sed -i 's/#host: "localhost:5601"/host: "localhost:5601"\n  username: "elastic"\n  password: "changeme"/g' packetbeat.yml 
```
