---

title: "docker compose for jenkins"

date: 2022-01-14

description: "docker compose for jenkins"

---

# docker-compose.yml

version: '3.7'  

services:  

jenkins:  

image: jenkins/jenkins:lts  

privileged: true  

user: root  

ports:  

\- 8083:8080  

\- 50003:50000  

container_name: my-jenkins-3  

volumes:  

\- ~/jenkins_data:/var/jenkins_home  

\- /var/run/docker.sock:/var/run/docker.sock

