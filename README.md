# Oracle_DB19c_Docker

MANUAL TO INSTALL ORACLE DATABASE 19c ON UBUNTU 22.04 WITH DOCKER:

1) create new directory e.g.:
sudo mkdir -p /var/oracle
cd /var/oracle

2) download Oracle images from oracle github repo (additional folders will be created):
git clone https://github.com/oracle/docker-images

3) download Oracle zip file from Oracle website (https://www.oracle.com/pl/database/technologies/oracle-database-software-downloads.html#19c)

4) Copy downloaded Oracle zip file into 19.3.0 folder (created during cloning Oracle repo):
cp <path-of-download-directory>/LINUX.X64_193000_db_home.zip /var/docker/docker-images/OracleDatabase/SingleInstance/dockerfiles/19.3.0/

5) Run docker -> go to dockerfiles folder and run buildContainerImage.sh file:
cd /var/docker/docker-images/OracleDatabase/SingleInstance/dockerfiles

sudo ./buildContainerImage.sh -v 19.3.0 -e

6) check if docker image has been created -> docker image with tag 19.3.0-ee should be displayed:
sudo docker images

7) create new directory where all oracle data will be stored:
sudo mkdir -p /u01/oracle

8) run docker image:
sudo docker  run --name oracle19c --network host -p 1521:1521 -p 5500:5500 -v /u01/oracle:/19.3.0 oracle/database:19.3.0-ee

the last line should be “XDB initialized”
then process might be ended by pressing: ctrl + c

9) start the oracle database:
sudo docker start oracle19c

10) set up your own password:
sudo docker exec oracle19c ./setPassword.sh <password>

11) Oracle Enterprise Manager Database Express is available under (proxy server might block an access -> webbrowser settings should be adjusted):
https://localhost:5500/em/shell

Username: system
Password: <password>
Container: orclpdb1

12) connect database from docker terminal -> sqlplus:
sudo docker exec -ti oracle19c sqlplus <username>/<password>@<container>
