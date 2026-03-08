# Jenkins server with one ssh agent

## Install required package
```
sudo apt install docker-ce docker-compose
```

## Create docker-compose file
```
version: '3.9'
 
services:
  jenkins:
    image: jenkins/jenkins:lts-jdk17
    restart: always
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins-data:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
 
  jenkins-agent:
    image: jenkins/inbound-agent:latest-alpine-jdk17
    restart: always
    init:  true
    command: "-url http://192.168.88.2:8080 4f1aed27ff2f38ec444acec96b2dcdd8dc7b48dc8239e9ee7ef2cd5b31f729bc agent1"
    volumes:
      - jenkins-agent-1:/home/jenkins/agent
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
 
volumes:
  jenkins-data:
  jenkins-agent-1:
```

## Run Jenkins Server
```
docker-compose up -d jenkins
```
Follow instructions

## add agent
on jenkins GUI add new agent with these information
- name: ssh-agent
- remot root direcotory: /home/jenkins/agent
- Launch method: choose "Launch agents vis ssh"

and save. from the page you can copy the key and paste into docker-compose.yaml

## Run agent
```
docker-compose up -d jenkins-agent
```


