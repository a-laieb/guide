# INSTALLATION GUIDE

# VM Requirement

 - CPU: `4 vCPU Cores`
 - RAM: `6 GB`
 - STORAGE: `25 GB`
 - OS: `UBUNTU 22.04`
   
# Connecting to the VPS

To connect your VPS server, you can use your server IP, you can create a root password and enter the server with your IP address and password credentials. But the more secure way is using an SSH key.
Creating SSH Key

### For MAC OS / Linux

1. Launch the Terminal app.
2. ```
   ssh-keygen -t rsa
   ```
3. Press `ENTER` to store the key in the default folder /Users/amine/.ssh/id_rsa).

4. Type a passphrase (characters will not appear in the terminal).

5. Confirm your passphrase to finish SSH Keygen. You should get an output that looks something like this:

```Your public key has been saved in /Users/amine/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:QFjMFz8flNR8Z6REPyWbuobETj7w4uif6bX9btQAykY amine@mac.local
The key's randomart image is:
+---[RSA 3072]----+
|     =o .. .o=+.o|
|    ..o ..E.o.o*=|
|      .. oo...++o|
|       . .+o o. .|
|        S.+ o  o |
|         B . .. .|
|        . B o.   |
|       o = =  .  |
|     .oo* . .+o  |
+----[SHA256]-----+
````

6. Copy your public SSH Key to your clipboard using the following code:

```
pbcopy < ~/.ssh/id_rsa.pub
```

## Connection

After copying the SSH Key go the to hosting service provider dashboard and paste your key and save. After,

```bash
ssh root@<server ip address>
```

## First Configuration

### Cleaning and updating server

```
apt clean all && sudo apt update && sudo apt dist-upgrade
```

```
rm -rf /var/www/html
```

### Installing Nginx

```
apt install nginx
```

### Installing and configure Firewall

```
apt install ufw
```

```
ufw allow "Nginx Full"
```

```
ufw allow 22
```

```
ufw allow 8800
```

```
ufw enable
```

## First Page

#### Delete the default server configuration

```
 rm /etc/nginx/sites-available/default
```

```
 rm /etc/nginx/sites-enabled/default
```

#### First configuration

```
nano /etc/nginx/sites-available/gfa
```

```
server {
root /var/www/gfa/;

    index index.html index.htm;

    location / {
    	proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    	try_files $uri.html $uri $uri/ /index.html;
    }

}

```

```
ln -s /etc/nginx/sites-available/gfa /etc/nginx/sites-enabled/gfa
```

##### Write your first message

```
nano /var/www/gfa/index.html
```

```
systemctl start nginx
```

##### Start Nginx and check the page

## Uploading Apps Using Git

```
apt install git
```

```
mkdir gfa
```

```
cd gfa
```

```
git git clone <your repository>
```

## Nginx Configuration for new apps

```
nano /etc/nginx/sites-available/gfa
```

##### If you check the location /api in your browser you are going to get "502" error which is good. Our configuration works. The only thing we need to is running our app

```
sudo apt install -y curl
```

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

```
cd api
```

```
npm install
```

```
nano .env
```

##### Copy and paste your env file
```
node index.js
```

#### But if you close your ssh session here. It's gonna kill this process. To prevent this we are going to need a package which is called

```
pm2
```

```
npm i -g pm2
```

Let's create a new pm2 instance

```
pm2 start --name api index.js
```

```
pm2 startup ubuntu
```

### React App Deployment

```
cd ../admin
```

```
nano .env
```

Paste your env file.

```
npm i
```

```
npm run build
```

Right now, we should move this build file into the main web file

```

rm -rf /var/www/gfa/\*

```

```

mkdir /var/www/gfa/admin

```

```

cp -r build/\* /var/www/gfa/admin

```

Let's make some server configuration

### Adding Domain

1 - Make sure that you created your A records on your domain provider website.

2 - Change your pathname from Router

3 - Change your env files and add the new API address

4 - Add the following server config

```

server {

    root /var/www/gfa/admin;


    index index.html;

    server_name admin.your_url;

    location / {

    	proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    	try_files $uri.html $uri $uri/ /index.html;
    	#try_files $uri $uri/ /index.html;
    }

}

server {

    root /var/www/gfa/employee;

    index index.html;

    server_name employee.your_url;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        try_files $uri.html $uri $uri/ /index.html;
        #try_files $uri $uri/ /index.html;
    }

}

server {

    root /var/www/gfa/kiosk;

    index index.html;

    server_name kiosk.your_url;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        try_files $uri.html $uri $uri/ /index.html;
        #try_files $uri $uri/ /index.html;
    }

}

server {

    root /var/www/gfa/screen;

    index index.html;

    server_name screen.your_url;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        try_files $uri.html $uri $uri/ /index.html;
        #try_files $uri $uri/ /index.html;
    }

}

server {

    root /var/www/gfa/feedback;

    index index.html;

    server_name feedback.your_url;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        try_files $uri.html $uri $uri/ /index.html;
        #try_files $uri $uri/ /index.html;
    }

}

server{

    server_name api.your_url;

    location / {

        proxy_cookie_domain ~^(.\*)$ "$1; Domain=your_url";
        proxy_set_header Host your_url;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #proxy_set_header X-Forwarded-Proto $scheme;
        #proxy_set_header Upgrade $http_upgrade;
        #proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;

        proxy_pass http://your_server_ip:8800;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

}

```

## SSL Certification

```
apt install certbot python3-certbot-nginx
```

```
certbot --nginx -d your_url -d www.your_url admin.your_url employee.your_url kiosk.your_url screen.your_url feedback.your_url
```

Let’s Encrypt’s certificates are only valid for ninety days. To set a timer to validate automatically:

```
systemctl status certbot.timer
```
