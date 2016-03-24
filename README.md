# docker-redmine

## Official redmine with mysql back-end

Requires :
* docker-engine version 1.10.2+
* docker-compose version 1.6.2+

## Usage
### Create directories on docker host
```
mkdir -p /data/redmine/mysql /data/redmine/db-init /data/redmine/files /data/redmine/plugins
```

### Pre-install redmine plugins (option)
Example :
```
cd /data/redmine/plugins
git clone https://github.com/annikoff/redmine_plugin_computed_custom_field.git computed_custom_field
git clone https://github.com/JostBaron/redmine_workload.git redmine_workload
curl -L https://bitbucket.org/bugzinga/redcase/downloads/redcase-1.0.zip > redcase-1.0.zip && unzip redcase-1.0.zip && rm -f redcase-1.0.zip
```

### Create then start containers
* `./run.sh` or `docker-compose up -d`

### Redmine database and plugins migrations
```
docker exec -it redmine_redmine_1 /bin/bash
```
```
cd /usr/src/redmine/
RAILS_ENV=production bundle exec rake db:migrate
RAILS_ENV=production bundle exec rake redmine:plugins:migrate
```

### Redmine URL
```
http://<docker_host_ip>
```

## Informations

### Volumes on docker host
* ``/data/redmine/mysql`` : database filesystem
* ``/data/redmine/db-init`` : database initialization (can contain a backup file xxx.sql.gz automatically restored at start of database container)
* ``/data/redmine/files`` : redmine user files
* ``/data/redmine/plugins`` : redmine plugins

### Environment
* ``REDMINE_NO_DB_MIGRATE=1`` : to prevent early redmine database migration
* ``PASSENGER_DOWNLOAD_NATIVE_SUPPORT_BINARY=1`` : to prevent passenger download
