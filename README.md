# FP Report

1 sudo apt update

2 sudo apt install apt-transport-https ca-certificates curl software-properties-common

3 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

4 echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

5 sudo apt update

6 sudo apt install docker-ce

7 sudo usermod -aG docker ${USER}

--

8 mkdir -p ~/.docker/cli-plugins/

9 curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

9.1 chmod +x ~/.docker/cli-plugins/docker-compose

--

10 sudo adduser bigipreport --shell=/bin/false

11 sudo mkdir -p /opt/bigipreport

12 cd /opt/bigipreport

13 sudo git clone https://github.com/DJR-FP/fp-status-page.git .

14 sudo chown -R bigipreport:bigipreport /opt/bigipreport

15 sudo chmod 755 /opt/bigipreport/underlay

16 sudo nano /opt/bigipreport/bigipreportconfig.xml

17 docker compose

mkdir fpreport

cd fpreport

nano docker-compose.yml

version: '3.3'
services:
    nginx:
        ports:
            - '8080:80'
        volumes:
            - '/var/run/docker.sock:/tmp/docker.sock:ro'
            - '/opt/bigipreport/underlay:/usr/share/nginx/html:ro'
        restart: 'always'
        logging:
            options:
                max-size: 1g
        image: nginx
        
sudo docker run --rm -v "/opt/bigipreport:/bigipreport/" bigipreport/data-collector

*/5 * * * * /usr/bin/docker run -d --rm -v "/opt/bigipreport:/bigipreport/" bigipreport/data-collector


