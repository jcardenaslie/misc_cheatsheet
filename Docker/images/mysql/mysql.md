
```sh
docker pull mysql

docker volume create mysql-data

docker run -d --name=mysql-server -p 3306:3306 -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1234 mysql
```