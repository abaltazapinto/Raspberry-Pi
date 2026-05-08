services:
  db:
    image: mariadb:11
    container_name: nc_db
    restart: unless-stopped
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    environment:
      MYSQL_ROOT_PASSWORD: rootpass123
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: nextcloud
      MYSQL_PASSWORD: ncpass123
    volumes:
      - ./db:/var/lib/mysql

  redis:
    image: redis:7
    container_name: nc_redis
    restart: unless-stopped
    volumes:
      - ./redis:/data

  app:
    image: nextcloud:latest
    container_name: nc_app
    restart: unless-stopped
    ports:
      - "8080:80"
    depends_on:
      - db
      - redis
    environment:
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: nextcloud
      MYSQL_PASSWORD: ncpass123
      MYSQL_HOST: db
      REDIS_HOST: redis
    volumes:
      - ./app:/var/www/html
      - ./data:/var/www/html/data
