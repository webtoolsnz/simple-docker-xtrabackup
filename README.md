# Simple Docker xtrabackup

The docker image simply runs xtrabackup to create mysql backups. It only preserve one previous backup.

## Use with docker-compose

The example can be found in `example` directory. Make sure you always mount the image's `/var/lib/mysql` to the mysql service's `/var/lib/mysql` directory.
Also you need to mount the backup directory to `/backups`.

Example `docker-compose.yml`:

```yaml
version: '3'
services:
  db:
    image: mariadb:5
    restart: always
    volumes:
      - db-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_PASSWORD}
    ports:
    - '3306:3306'
  xtrabackup:
    image: webtoolsdev/simple-docker-xtrabackup:latest
    environment:
      MYSQL_HOST: ${MYSQL_HOST}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - ./backups:/backups
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

To back up your database, you only need to run

```bash
docker-compose run --rm xtrabackup mysql-backup
```
