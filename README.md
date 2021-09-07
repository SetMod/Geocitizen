When *code* (see Download code) is *edited* (see Edit some properties) and package *installation* (see Install packages) ended, fix *bugs* (see Bugs) and do *Build*.

If dependencies are already installed, just do *Build.3*.
If there were any changes in project code after its previous Build, do *Build.3*.

# Build

1. mvn install
npm install
(npm audit fix)
npm run build
cp -r front-end/dist/* src/main/webapp/ - move all files from front-end/dist to src/main/webapp

2. In src/main/webapp/index.html:paste ‘.' characters (dots) before /static substrings after ‘<link href=’ and after three of ‘<script type=text/javascript src=’(there is a script  to do it written by : sed -i -E '//static/s/=/static/=./static/g' src/main/webapp/index.html)

3. mvn install
sudo mv target/citizen.war /var/lib/tomcat9/webapps/
sudo systemctl restart tomcat9

- The result is on {tomcat_host}:8080/citizen
- To check database for changes, do:
    psql -h 192.168.1.152 postgres -d ss_demo_1

## Download code

`git clone git@github.com:DevOpsAcademySS/Geocitizen.git` - clone repository
`cd Geocitizen` - go to repo folder
`git -b checkout A-72-kateryna-manual-deploy-geocitizen` - switch to branch of the ticket

## Install packages

- *node (v10.19.0)*
`sudo apt update`
`sudo apt install nodejs`
`nodejs -v / node -v`

- *npm (v6.14.4)*
`sudo apt update`
`sudo apt install npm`
`npm -v`

- *maven (v3.6.3, Java version: 11.0.11)*
`sudo apt update`
`sudo apt install maven`
`mvn -version`

- *java (v11.0.11) ([link](https://linuxize.com/post/how-to-install-tomcat-9-on-ubuntu-20-04/))*
`sudo apt install openjdk-11-jdk`
`sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat`

- *tomcat (v9) (same [link](https://linuxize.com/post/how-to-install-tomcat-9-on-ubuntu-20-04/))*
`sudo apt install tomcat9 tomcat9-admin`
`sudo systemctl enable tomcat9`
`sudo systemctl status tomcat9`
`sudo systemctl restart tomcat9`

## Bugs:

1. If there is no login button in right top of site, in front-end/package.json:
replace "vue-router": "^3.0.1"
with "vue-router": "3.0.1"

2. Also, there is a bug which resolves with:
replacing of "vue-material": "^1.0.0-beta-7"
with "vue-material": "1.0.0-beta-8"

Note:
If you want to make changes to frontend you have to cd to ~/Ch-058/front-end dir and run npm run dev. After successful execution you'll see url.

## Edit some properties (optionally)

**bug**: in pom.xml: change ‘http’ to ‘https’ in <repositories><repository><url>.

- In src/main/resources/application.properties:
replace ‘localhost’ with ip-address (where project is deploying) in variables front.url and front-end.url.
and replace ‘localhost’ with ip-address (where database is deployed) in variable db.url.

- In front-end/src/main.js:
replace 'localhost' with tomcat's ip (where project is deploying) in export const backEndUrl

- In front-end/config/index.js:
replace 'localhost' with tomcat's ip in host variable;
replace 8081 with 8080 in port variable (next after host var)

## Install and use psql on CentOS: 

- `sudo -u postgres psql -c "ALTER USER postgres WITH PASSWORD 'postgres';"`
- `sudo -u postgres psql -c 'create database ss_demo_1;'`
- `sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE ss_demo_1 TO postgres;"`

## Check DB connection

DB is on CentOS. Check from Ubuntu which is tomcat server:

- Install postgres on Ubuntu ([link](https://www.postgresql.org/download/linux/ubuntu/)):
`sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt  $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'`
`wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -`
`sudo apt-get update`
`sudo apt-get -y install postgresql`
`psql -h {DB_ip} postgres -d ss_demo_1` - connect to database ss_demo_1 by user postgres on ip DB_ip

## Other useful commands and links

Download pgAdmin4 v5.5 for Windows: https://www.postgresql.org/ftp/pgadmin/pgadmin4/v5.5/windows/

- For Ubuntu
npm cache clean --force
sudo ufw allow
sudo ufw enable
sudo ufw status verbose
Tomcat: 
https://linuxhint.com/install_apache_tomcat_server_ubuntu/
or 
Use https://downloads.apache.org/tomcat/tomcat-9/v9.0.52/bin/apache-tomcat-9.0.52.tar.gz in https://linuxize.com/post/how-to-install-tomcat-9-on-ubuntu-20-04/
Open port:
sudo ufw allow from any to any port 8080 proto tcp
Ping host from text web-browser:
sudo apt install w3m
w3m http://127.0.0.1:8080/ 

https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart-ru

- For CentOS
sudo -u postgres psql\q\conninfo
“FirewallD is not running”:  https://stackoverflow.com/questions/63085297/firewalld-is-not-running
https://winitpro.ru/index.php/2019/09/26/ustanovka-postgresql-db-centos/
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-centos-7
