# blackbox

INSTALLATION PROCESS ONLINE
SIGNALK

domain: e-ship.info
server: digitalocean.com VPS - Ubuntu 18.04.3 (LTS) x64
https://github.com/signalk/signalk-server-node/blob/master/raspberry_pi_installation.md
https://github.com/signalk/signalk-server-node

CONSOLE COMMANDS DURING THE INSTALLATION PROCESS:

#1: INSTALL DEPENDENCIES
         UPDATE THE REPOSITORIES
                        $ sudo apt update
 	NODE+NVM
                        $ sudo apt install nodejs npm
         MAKE SURE IT IS LATEST VERSION OF NPM
                        $ sudo npm install -g npm@latest
                       
#2: INSTALL SIGNAL K
         INSTALL THE NODE SERVER USING NPM
                        $ sudo npm install -g --unsafe-perm signalk-server
                        RECOMMENDED: Create a VPS snapshot on this point
         START THE NODE SERVER
                        $ signalk-server --sample-nmea0183-data …OR… $ signalk-server --sample-n2k-data
                        * signalk-server running at 0.0.0.0:3000 output should display

#3: OWN SETUP AND RUNNING AUTOMATICALLY AS DEAMON
                        $ sudo signalk-server-setup
                        NOTE: After running the command above and SSL set to ‘on’ - https connection doesn’t work


#4: SETUP TO WORK WITH NMEA 0183 / NMEA 2000 + SAMPLE NMEA 0183 / NMEA 2000 OUTPUT:
         https://github.com/signalk/signalk-server-node/blob/master/raspberry_pi_installation.md#real-inputs
	https://github.com/signalk/signalk-server-node/blob/master/raspberry_pi_installation.md#sample-files
	               $ sudo find / -name "aava-n2k.data"
                        $ sudo find / -name "plaka.log"

#5: UPDATE TO LATEST (once a while)
	https://github.com/signalk/signalk-server-node/blob/master/raspberry_pi_installation.md#updating-your-raspberry-pi-and-signal-k-node-server

INFLUXDB

#1: INSTALL THE INFLUXDB
         https://docs.influxdata.com/influxdb/v1.8/introduction/install/
                        $ curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -
                        $ source /etc/lsb-release
                        $ echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
                        $ sudo apt-get update && sudo apt-get install influxdb
                        $ sudo service influxdb start

#2: INSTALL THE PLUGIN
         https://github.com/tkurki/signalk-to-influxdb
         https://www.npmjs.com/package/signalk-to-influxdb
         http://e-ship.info:3000/admin/#/appstore/apps (find + install)
         ctrl+c in the console to quit the current process in order to restart
                        $ signalk-server --sample-nmea0183-data …OR… $ signalk-server --sample-n2k-data

#3: SETUP THE PLUGIN
         http://e-ship.info:3000/admin/#/serverConfiguration/plugins/signalk-to-influxdb
                        $ curl -X POST http://localhost:8086/query?q=CREATE+DATABASE+boatdata

#4: USEFUL COMMANDS
          $ influx (go to influx database)
          $ CREATE DATABASE boat data
          $ SHOW DATABASES
          $ exit

GRAFANA

#1: INSTALL THE GRAFANA
         https://grafana.com/docs/grafana/latest/installation/debian/

         https://grafana.com/grafana/download?platform=linux&edition=enterprise
                  $ sudo apt-get install -y adduser libfontconfig1
                  $ wget https://dl.grafana.com/enterprise/release/grafana-enterprise_7.1.3_amd64.deb
                  $ sudo dpkg -i grafana-enterprise_7.1.3_amd64.deb


         http://e-ship.info:3000/login
         admin / 14Archiepoo

         NOTE: By default Grafana installation overwrites SignalK, simply because it occupies the same port (3000).
         To have access to SignalK AND Grafana after Grafana installation, change Grafana port:
         Open /etc/grafana/grafana.ini and change http_port: remove the ';' and change to 3001 or else.
         $ sudo service grafana-server restart
