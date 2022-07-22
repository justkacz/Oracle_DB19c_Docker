# Oracle_DB19c_Docker

MANUAL TO INSTALL ORACLE DATABASE 19c ON UBUNTU 22.04 WITH DOCKER:

## 1) create new directory e.g.:
sudo mkdir -p /var/oracle
<br>
cd /var/oracle
<br>
## 2) download Oracle images from oracle github repo (additional folders will be created):

git clone https://github.com/oracle/docker-images
<br>
## 3) download Oracle zip file from Oracle website (https://www.oracle.com/pl/database/technologies/oracle-database-software-downloads.html#19c)
## 4) Copy downloaded Oracle zip file into 19.3.0 folder (created during cloning Oracle repo):
cp \<path-of-download-directory\>\/LINUX.X64_193000_db_home.zip /var/docker/docker-images/OracleDatabase/SingleInstance/dockerfiles/19.3.0/
<br>
## 5) Run docker -> go to dockerfiles folder and run buildContainerImage.sh file:
cd /var/docker/docker-images/OracleDatabase/SingleInstance/dockerfiles
<br>
sudo ./buildContainerImage.sh -v 19.3.0 -e
<br>
## 6) check if docker image has been created -> docker image with tag 19.3.0-ee should be displayed:
sudo docker images
<br>
## 7) create new directory where all oracle data will be stored:
sudo mkdir -p /u01/oracle
<br>
## 8) run docker image:
sudo docker  run --name oracle19c --network host -p 1521:1521 -p 5500:5500 -v /u01/oracle:/19.3.0 oracle/database:19.3.0-ee

<br>
the last line should be “XDB initialized”
<br>
then process might be ended by pressing: ctrl + c
<br>
 - v = VOLUME -> maps a volume to the container so that the data stored by the container is stored at /u01/oracle on the host (all data will be available when container is removed)

## 9) start the oracle database:
sudo docker start oracle19c
<br>
## 10) set up your own password:
sudo docker exec oracle19c ./setPassword.sh \<new_password\>
<br>
## 11) Oracle Enterprise Manager Database Express is available under (proxy server might block an access -> webbrowser settings should be adjusted):
https://localhost:5500/em/shell
<br>
Username: system
<br>
Password: \<new_password\>
<br>
Container: orclpdb1
<br>
## 12) connect database from docker terminal -> sqlplus:
sudo docker exec -ti oracle19c sqlplus <username\>\/\<password\>\@\<container\>
