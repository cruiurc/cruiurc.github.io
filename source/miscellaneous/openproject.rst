Open Project docker部署
========================

::

    1  2023-10-27 09:32:10
    2  2023-04-29 09:25:44 ls
    3  2023-04-29 09:25:59 curl -fsSL https://get.docker.com -o get-docker.sh
    4  2023-04-29 09:26:05 sh get-docker.sh
    5  2023-04-29 09:26:58 sudo apt install docker-compose -y
    6  2023-04-29 09:28:08 sudo apt install git
    7  2023-04-29 09:28:17 git clone https://github.com/opf/openproject-deploy --depth=1 --branch=stable/12 openproject
    8  2023-04-29 09:28:25 cd openproject/compose
    9  2023-04-29 09:28:30 docker-compose pull
    10  2023-04-29 09:28:39 docker-compose pull
    11  2023-04-29 09:28:53 systemctl start docker
    12  2023-04-29 09:29:00 systemctl status docker
    13  2023-04-29 09:29:21 docker-compose pull~
    14  2023-04-29 09:29:25 systemctl start docker
    15  2023-04-29 09:29:34 docker-compose pull
    16  2023-04-29 09:31:49 docker-compose pull
    17  2023-04-29 09:32:03 OPENPROJECT_HTTPS=false docker-compose up -d
    18  2023-10-27 09:32:42 history

作为今后部署的参考。
