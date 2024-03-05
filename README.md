Task:
Create a Docker Compose file for n8n, incorporating a connection to a database named "bossgrasser." Additionally, configure n8n to reference a JavaScript file ("hello-world.js") within the n8n environment. Ensure the integration is seamless and professional.

Docker 1 Hr learning https://www.youtube.com/watch?v=pTFZFxd4hOI
Installing Docker
--------------------------------------------------------------------
   01. Log into the Linux based devicee
   02. Run the following commands in the terminal
         install prerequisites
         sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg-agent -y
         add docker gpg key
         curl -fsSL https://download.docker.com/linux/$(awk -F'=' '/^ID=/{ print $NF }' /etc/os-release)/gpg | sudo apt-key add -
         add docker software repository
         sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/$(awk -F'=' '/^ID=/{ print $NF }' /etc/os-release) $(lsb_release -cs) stable"
         install docker
         sudo apt install docker-ce docker-compose containerd.io -y
         enable and start docker service
         sudo systemctl enable docker && sudo systemctl start docker
         add the current user to the docker group
         sudo usermod -aG docker $USER
         reauthenticate for the new group membership to take effect
         su - $USER
 
--------------------------------------------------------------------
Running the n8n Container
--------------------------------------------------------------------
   01. Now that Docker is installed, run the following commands to setup the n8n Docker container and run it
         create working directory
         mkdir ~/docker/n8n -p && mkdir ~/docker/mariadb
         set ownership on the working directories
         sudo chown "$USER":"$USER" ~/docker -R
         run the mariadb docker container
         docker run -d --name mariadb -e MYSQL_ROOT_PASSWORD=r00tp@$$ -e MYSQL_USER=n8n_rw -e MYSQL_PASSWORD='n8n_N8N!' -e MYSQL_DATABASE=n8n_db -v ~/docker/mariadb:/var/lib/mysql -p 3306:3006 -d mariadb:latest
         run the n8n container
         docker run -d --name=n8n --link mariadb -p 5678:5678 -v ~/docker/n8n:/home/node/.n8n -e GENERIC_TIMEZONE=America/New_York -e TZ=America/New_York -e DB_TYPE=mysqldb -e DB_MYSQLDB_DATABASE=n8n_db -e DB_MYSQLDB_HOST=mariadb -e DB_MYSQLDB_PORT=3306 -e DB_MYSQLDB_USER=n8n_rw -e DB_MYSQLDB_PASSWORD='n8n_N8N!' n8nio/n8n
   02. Open a web browser and navigate to http://DNSorIP:5678
   03. Complete the form with an email, first name, last name and password ≫ Click next
   04. Complete the questionnaire ≫ Click continue
   05. Click Get started
   06. Welcome to n8n
 
Documentation:  https://hub.docker.com/r/n8nio/n8n