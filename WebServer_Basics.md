# Web Server

1. Web Server --> Serves UI or TML/CSS. Communicates with Application server
2. Application Server --> Servers business logic


## Apache Web Server

1. Web Server, Open source. Runs with Application server for getting business logic. Used to server HTML/CSS/JS static. Default Port : 80
2. Run the server using `systemctl start httpd`
3. Conf File : `/etc/httpd/conf/httpd.conf`
4. ServerName specifies the Hostname for your website : `#ServerName www.example.com:80`
5. Listen specifies the Port to listen on : `Listen 80`
6. Document Root Can be given using : `DocumentRoot "/var/www/html"`
7. Virtual Host are used to configure multiple apps in single web server
  ```
  <VirtualHost *:80>
    ServerName www.organges.com
    DocumentRoot /var/ww/oranges
  </VirtualHost>
  <VirtualHost *:80>
    ServerName www.apples.com
    DocumentRoot /var/ww/apples
  </VirtualHost>
  ```
8. or move the configuration to separate config file and include those files `Include conf/apples.conf`


## Apache Tomcat

1. Provides web server to host JAVA Applications
2. Default Port : 8080
3. Download tomcat extract from BIN file in the link https://downloads.apache.org/tomcat/tomcat-8/v8.5.82/bin/
4. Download tar using `curl -O https://downloads.apache.org/tomcat/tomcat-8/v8.5.82/bin/apache-tomcat-8.5.82.tar.gz`
5. Extract the Tar using `tar -xvf apache-tomcat-8.5.82.tar.gz`
6. Move the extracted files to opt using `sudo mv apache-tomcat-8.5.82 /opt/apache-tomcat-8`
7. Run the Tomcat : `sudo /opt/apache-tomcat-8/bin/startup.sh`
8. To edit port : Change `/opt/apache-tomcat-8/conf/server.xml` Connector for HTTP server
9. Shutdown and start the server again `sudo /opt/apache-tomcat-8/bin/shutdown.sh;sudo /opt/apache-tomcat-8/bin/startup.sh`
10. Place the application in `/opt/apache-tomcat-(versionNumber)/webapps` to be served



## Flask - Python Server

1. Install dependencies `pip install -r requirements.txt`
2. Run Flash app `python main.py`
3. To deploy Flask in production, we should use production grade webserver
4. Using gunicorn : `gunicorn main:app -w 2` Listens on 8000, running two workers (threads) . main:app -> {main} is the file name for start


## NodeJS

1. Install dependencies `npm install`
2. Run the app `npm run {script}`
3. Process Manager helps to manage run in production mode. It has built in load balancer to reduce unavailability during crash `pm2 start app.js -i 4` 4 instances of server



## IP addresses and Ports

1.  
