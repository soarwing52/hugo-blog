---

title: "Windows Docker 安裝 Gitlab"

date: 2021-07-15

description: "Windows Docker 安裝 Gitlab"

tags:
  - docker
  - gitlab
  - Work

---

最近公司要推使用gitlab來做自動build+發布，於是我就拿我這邊的raspberry pi作為發布機器

在電腦上安裝gitlab用區網作為pipeline建置測試，因為目前還沒有放AWS

heroku之類的雲端託管，原因的話，在前幾篇試過，主要是postgres還需要一點錢 除非再把網站改回舊版

那就用docker compose 安裝吧

我用 [官網教學](https://docs.gitlab.com/ee/install/docker.html#install-gitlab-using-

docker-compose) 基本上就完成了

    

    

    web:

      image: 'gitlab/gitlab-ee:latest'

      restart: always

      hostname: 'gitlab.example.com'

      environment:

        GITLAB_OMNIBUS_CONFIG: |

          external_url 'https://gitlab.example.com'

          # Add any other gitlab.rb configuration here, each on its own line

      ports:

        - '80:80'

        - '443:443'

        - '22:22'

      volumes:

        - '$GITLAB_HOME/config:/etc/gitlab'

        - '$GITLAB_HOME/logs:/var/log/gitlab'

        - '$GITLAB_HOME/data:/var/opt/gitlab'

我把 $GITLAB_HOME的變數直接用 . 就是我放docker compose的資料夾

然後用community edition 其實我有看到建議一開始就用ee 因為要升級的時候不用重新再build一次 不過我這確定免錢仔就這樣了

所以我的docker compose 長這樣

    

    

    web:

      image: 'gitlab/gitlab-ce:latest'

      restart: always

      hostname: 'gitlab.example.com'

      environment:

        GITLAB_OMNIBUS_CONFIG: |

          external_url 'https://gitlab.example.com'

          # Add any other gitlab.rb configuration here, each on its own line

      ports:

        - '80:80'

        - '443:443'

        - '22:22'

      volumes:

        - './config:/etc/gitlab'

        - './logs:/var/log/gitlab'

        - './data:/var/opt/gitlab'

然後 密碼的話網路很多人找不到，我一開始也看log不知道在哪裡

其實就在config裡面的initial_root_password

然後要改密碼就 docker exec -it containername bash

進去之後 sudo gitlab-rake "gitlab:password:reset"

就可以重新設定密碼了

搭建時間有點久，需要一點耐心之外，這件事情基本上就很簡單，接下來就開始發布啦

