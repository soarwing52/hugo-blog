---

title: "docker compose dotnetMVC與API + nginx"

date: 2021-09-08

description: "docker compose dotnetMVC與API + nginx"

---



既然上一篇是把專案的docker image做好，這邊就是把它發布



如果直接發布的話，沒甚麼問題，就是直接放PORT



不過既然沒有用IIS，就加個nginx，算是這次要加上的新工具吧



這邊就直接先放上docker compose



    

    

    version: "3.9"  # optional since v1.27.0

    services:

      frontend:

        container_name: nginx

        image: nginx:latest

        volumes:

          - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro

        ports:

          - "8080:80"

        restart: always

        networks:

          - some-net

    

      ttweb:

        # image: ttweb

        build: ./UMATTUrbanSystem

        depends_on:

          - frontend

        container_name: ttweb

        expose:

        - "5000"

        restart: always

        networks:

          - some-net

    

      ttapi:

        # image: ttapi

        build: ./UMATTUrbanAPI

        container_name: ttapi

        expose:

          - "9090"

        networks:

          - some-net

    

    networks:

      some-net:

        driver: bridge

    

    



先講ttweb&ttapi兩個，就是上一篇的image做出來，用save就可以，是我註解掉的image: ttapi



build就是在docker compose build的時候(或是如果沒有build 直接up)的時候就會用dockerfile跑起來



然後上一篇提到的 expose就是直接把host跟guest的9090打通，如果docker run --scale=n就會跑起n個容器來分散流量



然後要讓web跟api互相能連，如果沒有nginx，就只要互相給port



今天是把入口只有nginx然後再互連，就要用networks



其實我自己也是看nginx docker compose之後抄來的



nginx config



    

    

    worker_processes 1;

    

    events { worker_connections 1024; }

    

    http {

    

        sendfile on;

    

        upstream ttwebs {

            server ttweb:5000;

        }

    

        server {

            listen 80;

            # server_name $hostname;

            server_name localhost 127.0.0.1;

            location / {

                proxy_pass         http://ttwebs;

                proxy_redirect     off;

                proxy_http_version 1.1;

                proxy_cache_bypass $http_upgrade;

                proxy_set_header   Upgrade $http_upgrade;

                proxy_set_header   Connection keep-alive;

                proxy_set_header   Host $host;

                proxy_set_header   X-Real-IP $remote_addr;

                proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;

                proxy_set_header   X-Forwarded-Proto $scheme;

                proxy_set_header   X-Forwarded-Host $server_name;

            }

    

            location /ttweb/ {

                proxy_pass         http://ttweb:5000/;

            }

    

            location /ttapi/ {

                proxy_pass http://ttapi:9090/api/Announcement/Committee;

            }

            location /string/ {

                return 200 'a string response';

            }

        }

    }



這樣就是 主機IP/ttweb 就會連到這邊host的5000的port連到docker 就是ttweb



然後我再ttapi先接連到一個get 讓我確定可以直接得到API的結果



結語：



nginx的流量附載還是一個坑 還不會



不過這次能把nginx用network串起來就已經是一個階段性目標了



中間卡很久的的是怎麼都連不起來 不過我重新build一次nginx就好了



