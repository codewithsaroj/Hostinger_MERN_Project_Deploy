# Hostinger_MERN_Project_Deploy

Deploying MERN Stack Project on Hostinger VPS
==============================================

Step 1 : Setup Node Js on VPS
=======================================
    1)  Log in to Your VPS in Terminal
        > ssh root@your_vps_ip
 
    2) Update and Upgrade Your System
        > sudo apt update
        > sudo apt upgrade -y
  
    3) Install Node.js using NVM
    To install or update nvm, you should run the install script. To do that, you may either download and run the script manually, or use the following cURL or Wget command:

        > curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
                or 
        > wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
  

    4) Install node 
        > nvm install <node-version>
        > nvm install --lts
    5) Check Node version
        > node -v
        > npm -v

Step 2 : Setup MongoDB on VPS
=============================
    1) Install MongoDB
        > sudo apt-get install gnupg curl
        > curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
    sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
    --dearmor
        > echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
        > sudo apt-get update
        > sudo apt-get install -y mongodb-org
        > sudo systemctl start mongod
        > sudo systemctl daemon-reload
        > sudo systemctl status mongod
        > sudo systemctl enable mongod
        > sudo systemctl stop mongod
        > sudo systemctl restart mongod

        url => mongodb://127.0.0.1:27017
  
    Configure MongoDB Authentication ( Optional )
    Allowing MongoDB through firewall Enable firewall

        > sudo ufw enable
    Check Port is Allowed through Firewall

        > sudo ufw status
    If Port is not Allowed then Allow it through Firewall
        > sudo ufw allow 27017
        > sudo ufw allow 'OpenSSH'
    Restart MongoDB
        > sudo service mongod restart
    Secure MongoDB by setting up Super User.
    Connect to MongoDB shell

        > mongosh
        Show database

        > show dbs
    Change to admin database

        > use admin
    Create superuser with all privileges

        > db.createUser({user: "username-here" , pwd: passwordPrompt() , roles: ["root"]})
    Now Exit mongosh shell

        > .exit
    Enable Authorization removing comment

        > sudo nano /etc/mongod.conf
    security: authorization: enabled
        > Restart MongoDB
        > sudo service mongod restart

    To Access Mongo Shell as Super User use this command:
        > mongosh --port 27017 --authenticationDatabase "admin" -u "username-here" -p "password-here"
    Create Database & User for project:
        > use database_name
        > db.createUser({user:"username_here", pwd:passwordPrompt(), roles:[{role:"readWrite", db:"database_name"}]})
    Now Exit mongosh shell

        > .exit
    MongoDB URI to Connect with projects:  mongodb://username-here:password-here@127.0.0.1:27017/database_name


Step 3 : Setup Git on VPS
=============================

    1)   Install Git
        > sudo apt install -y git
    2)  > sudo apt update
    3)  > sudo apt install gh
    4)  > gh auth login
    5)  > mkdir /var/www/sutraa_events
    6)  > cd /var/www/sutraa_events
    7)  > git clone https://github.com/codewithsaroj/event-crm-backend.git and  git clone https://github.com/codewithsaroj/event-crm-frontend.git
    8)  > cd event-crm-backend & npm install
    9)  > nano .env and change the environment variables
    10) > cat .env
        FRONTEND_URL = https://frontend.com
        PORT = 7000
        MONGO_URI = mongodb://127.0.0.1:27017/db_name
        HASH_SALT = 10
        JWT_SECRET_KEY = uihreurewrueruerewurew
        CLOUDINARY_CLOUD_NAME = fjfhd
        CLOUDINARY_API_KEY = 7347374745
        CLOUDINARY_API_SECRET = fdkfdahh8438432

    11) >root@srv684955:/var/www/event_project/event-crm-backend#  npm install -g pm2
    12) >root@srv684955:/var/www/event_project/event-crm-backend# npm run start
    13) >root@srv684955:/var/www/event_project/event-crm-backend#  pm2 start app.js --name event_backend
    14) >root@srv684955:/var/www/event_project/event-crm-backend#  pm2 start src/app.js --name event_backend
    15) >root@srv684955:/var/www/event_project/event-crm-backend# pm2 startup
    16) root@srv684955:/var/www/event_project/event-crm-backend# pm2 save
    > root@srv684955:/var/www/event_project/event-crm-backend#  sudo ufw status
    Status: inactive
>root@srv684955:/var/www/event_project/event-crm-backend#  sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
    > root@srv684955:/var/www/event_project/event-crm-backend#  sudo ufw status
Status: active
    > root@srv684955:/var/www/event_project/event-crm-backend#  sudo ufw allow 'OpenSSH'
Rule added
Rule added (v6)
    > root@srv684955:/var/www/event_project/event-crm-backend# sudo ufw allow 7000
Rule added
Rule added (v6)
    > root@srv684955:/var/www/event_project/event-crm-backend#  sudo ufw status
Status: active

    > root@srv684955:/var/www/event_project/event-crm-backend# cd ..
    > root@srv684955:/var/www/event_project# cd event-crm-frontend/
    > root@srv684955:/var/www/event_project/event-crm-frontend# npm install
    > root@srv684955:/var/www/event_project/event-crm-frontend# nano .env
    > root@srv684955:/var/www/event_project/event-crm-frontend# cat .env
    > root@srv684955:/var/www/event_project/event-crm-frontend# npm install
    > root@srv684955:/var/www/event_project/event-crm-frontend# npm run build
    > root@srv684955:/var/www/event_project/event-crm-frontend#  sudo apt install -y nginx

    > root@srv684955:/var/www/event_project/event-crm-frontend# sudo ufw status
    > root@srv684955:/var/www/event_project/event-crm-frontend#  sudo ufw allow 'Nginx Full'

    > root@srv684955:/var/www/event_project/event-crm-frontend# sudo ufw status
        Status: active
    > nano /etc/nginx/sites-available/event.codewithsaroj.online.conf

 server {
    listen 80;
    server_name event.codewithsaroj.online www.event.codewithsaroj.online;

    location / {
        root /var/www/event_project/event-crm-frontend/dist;
        try_files $uri /index.html;
    }
}


    > ln -s /etc/nginx/sites-available/event.codewithsaroj.online.conf /etc/nginx/sites-enabled/

# Test the Nginx configuration for syntax errors.

    > nginx -t
    > systemctl restart nginx

    > nano /etc/nginx/sites-available/api.codewithsaroj.online.conf

server {
    listen 80;
    server_name api.codewithsaroj.online;

    location / {
        proxy_pass http://localhost:7000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

    > ln -s /etc/nginx/sites-available/api.codewithsaroj.online.conf /etc/nginx/sites-enabled/

    > systemctl restart nginx

    # Connect Domain Name with Website
    # Point all your domain & sub-domain on VPS IP address by adding DNS records in your domain manager

    # Now your website will be live on domain name

    # Setting Up SSL Certificates Install Certbot

    > sudo apt install -y certbot python3-certbot-nginx
    # Obtain SSL Certificates

    > certbot --nginx -d event.codewithsaroj.online -d  www.event.codewithsaroj.online -d api.codewithsaroj.online
    # Verify Auto-Renewal

    > certbot renew --dry-run




