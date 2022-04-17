install ubuntu 20.04
////
sudo -i
apt update
apt upgrade
snap install docker         
apt install docker-compose
git clone https://github.com/kankankuli/13.0.git
cd 13.0
docker build -t 13.0/odoo13:20211007 .
cd\
nano docker-compose.yml
###

version: '2'
services:
  db:
    image: postgres:latest
    environment:
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
      - POSTGRES_DB=postgres
    restart: always             # run as a service
    volumes:
        - ./postgresql:/var/lib/postgresql/data

  odoo13:
    image: 13.0/odoo13:20211007
    depends_on:
      - db
    ports:
      - "8069:8069"
    tty: true
    command: -- --dev=reload

    volumes:
      - ./addons:/mnt/extra-addons
      - ./etc:/etc/odoo
    restart: always             # run as a service
    
    #####

docker-compose up -d
nano etc/odoo.conf

###
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo
;logfile = /var/log/odoo/odoo.log
admin_passwd = Lsf7000+
;list_db = False
list_db = True
###
git clone https://github.com/kankankuli/addons.git
run server
create data

wget https://www.soladrive.com/downloads/enterprise-13.0.tar.gz
tar -zxvf enterprise-13.0.tar.gz
mv -v ~/13.0/* ~/addons/
docker-compose stop
docker-compose up -d
