# loggy-server

# What is it?

A standard Grafana/Loki/Promtail stack for forwarding logs to a central location for viewing. 

It is simply a git repo with a docker-compose file and a bunch of pre-created directories with config files in them. You can just clone the repo, create some necessary files (certificates, httpauth) and run docker-compose up. 

# Install

 Prerequisites 
- Create a new VM. Ubuntu server works best as it was used to make this guide
-  Make sure the hostname of the VM is set. I like to pick "loggy"


1. Install docker
```bash
sudo apt install docker.io -y
```
   
If using a proxy, configure docker to use it
```bash
sudo systemctl edit docker.service
```

```
[Service]
Environment="HTTP_PROXY=http://199.100.16.100:3128"
Environment="HTTPS_PROXY=http://199.100.16.100:3128"
Environment="NO_PROXY=localhost,127.0.0.1"
```
   
```bash
sudo systemctl restart docker
```
   
2. Install docker-compose
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.11.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
   
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
   
3. isntall apache2-utils
```bash
sudo apt install apache2-utils
```
   
4. Clone the repo
```bash
git clone https://github.com/Corbeno/loggy-server
```
   
```bash
cd loggy-server
```
   
5. Create a self-signed cert for nginx
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx/certificates/nginx.key -out nginx/certificates/nginx.crt
```

> Note: You can skip all questions it asks except "Common Name", where you should put the IP address of this machine


6. Create a password for http auth in nginx
```bash
sudo htpasswd -c nginx/passwords loggy
```

> Note: You can change "loggy" to whatever you want, as long as it matches in the .env file

  
7. Update .env file and update anything with "TODO" next to it
```bash
vim .env
```
   
8. Lock down .env file
```bash
sudo chmod 660 .env
```
   
9. start the service
```bash
docker-compose up -d
```
> Note: You can run the above command without -d to see the live output for debugging



# Notes
Any passwords that are required to run should be put in .env. Any other config file should be considered readable by anyone. 

