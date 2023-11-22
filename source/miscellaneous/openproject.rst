Open Project docker部署
========================
部署参考:

::

    # 确认openproject.sql文件存在于~
    sudo apt update
    sudo apt upgrade
    sudo apt autoremove
    sudo apt install curl
    curl -fsSL https://get.docker.com -o get-docker.sh
    sh get-docker.sh
    sudo apt install docker-compose -y
    sudo apt install git
    git clone https://github.com/opf/openproject-deploy --depth=1 --branch=stable/13 openproject
    mkdir -p /var/lib/openproject/{pgdata,assets}
    systemctl start docker
    docker run --rm -v /var/lib/openproject/pgdata:/var/openproject/pgdata -it openproject/community:13
    docker run --rm -d --name postgres -v /var/lib/openproject/pgdata:/var/lib/postgresql/data postgres:13
    docker cp openproject.sql postgres:/
    docker exec -it postgres psql -U postgres

    # In psql you then restore dump like this:
    DROP DATABASE openproject;
    CREATE DATABASE openproject OWNER openproject;

    \c openproject
    \i openproject.sql

    # Once this has finished you can quit psql (using \q) and the container (exit).

    cd openproject/compose
    docker-compose pull

    docker run -d -it -p 8080:80 -p 5432:5432 --name openproject \
      -e OPENPROJECT_HOST__NAME=106.54.65.45:8080 \
      -e OPENPROJECT_SECRET_KEY_BASE=secret \
      -e OPENPROJECT_HTTPS=false \
      -e OPENPROJECT_DEFAULT__LANGUAGE=zh-CN \
      -v /var/lib/openproject/pgdata:/var/openproject/pgdata \
      -v /var/lib/openproject/assets:/var/openproject/assets \
      openproject/community:13
