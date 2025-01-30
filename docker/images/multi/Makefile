DB = "librebooking"

# allow multiline target to keep all commands in one shell
.ONESHELL:

up:
	cd docker
	cp ../docker-compose.yml .
	docker-compose up -d
	cd ..
	docker cp ./my.cnf librebooking-db:/root/.my.cnf
	docker cp run-db-backup librebooking-db:/usr/local/bin/
	docker cp list-databases librebooking-db:/usr/local/bin/
	docker cp list-tables librebooking-db:/usr/local/bin/
	docker cp describe-table librebooking-db:/usr/local/bin/
	docker cp show-table-data librebooking-db:/usr/local/bin/
	docker cp run-app-backup librebooking:/usr/local/bin

# get db config file
get-db-config:
	docker cp librebooking-db:/config/custom.cnf ./custom.cnf

# put db config file
put-db-config:
	docker cp custom.cnf librebooking-db:/config/custom.cnf

# get app config file
get-app-config:
	docker cp librebooking:/config/config.php ./config.php

# put app config file
put-app-config:
	docker cp config.php librebooking:/config/config.php
	docker exec -it librebooking bash chmod -R 0777 /config/config.php

# shutdown everything
down:
	cd docker
	docker-compose down

# easier 
restart:
	make down
	make up

# open a docker shell to the database instance
dbshell:
	docker exec -it librebooking-db bash

# open a docker shell to the php instance
appshell:
	docker exec -it librebooking bash

# get docker status of our libre processes
status:
	docker ps -f "name=libre*"

# list the databases on the database instance
list-db:
	docker exec -it librebooking-db list-databases

# list the tables on the database instance
list-tb:
	docker exec -it librebooking-db list-tables $(DB)

# list the table data on the database instance
show-tb:
	docker exec -it librebooking-db show-table-data $(DB) $(table)

# run db backup
db-backup:
	docker exec -it librebooking-db run-db-backup

# run app backup
app-backup:
	docker exec -it librebooking run-app-backup

