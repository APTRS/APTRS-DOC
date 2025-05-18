# Manual Installation

If you prefer to install the APTRS without Docker and have more control over each service, such as Redis, Postgresql, Nginx, etc., or if you want to host the database on a different server, Redis on a different server, etc., you can follow the below process for deploying the APTRS on a server without Docker.

The steps mentioned below were tested on the below setup:

- OS - Ubuntu 24.10
- Python - 3.12.7  (3.9+ is required)
- Python Poetry - 1.8.4
- PostgreSQL - 16.4 (Ubuntu 16.4-1build1)
- Redis Server - 7.0.15
- Nginx - 1.26.0
- NodeJS - 20.16.0 (18+ is required)
- NPM - 9.2.0

!!! info "Note"

    While we have provided detailed steps for manual installation and deployment, please note that this method is more prone to errors. Variations in software versions, updates, system configurations, and other factors may result in unexpected issues. Manual installation is intended for individuals who have a solid understanding of Linux, some level of development experience, or expertise in application deployment, and are comfortable troubleshooting potential errors that may arise.

## Reference Links

- [Digital Ocean - How To Set Up Django with Postgres, Nginx, and Gunicorn on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu#step-10-configure-nginx-to-proxy-pass-to-gunicorn)
- [Digital Ocean - How To Install Nginx on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)
- [Digital Ocean - How To Secure Nginx with Let's Encrypt on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04)
- [Digital Ocean - How To Install and Secure Redis on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-22-04)
- [Digital Ocean - How To Install and Use PostgreSQL on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04)

## PostgreSQL Setup

Run the below command to install PostgreSQL.

```bash
sudo apt-get install  postgresql postgresql-contrib
```

After the installation is complete, we can access the PostgreSQL shell using the command below.

```bash
sudo -u postgres psql
```

From the PostgreSQL shell, create a new database for the APTRS project. Change the name to something relevant to the project.

```bash
CREATE DATABASE aptrsdb;
```

Next, create a database user for our project. Make sure to select a secure password:

```bash
CREATE USER aptrsdb_user WITH PASSWORD 's!!D%AriPB-MO~5';
```

We will set the default encoding to UTF-8 and adjust other settings. Please update the time zone according to your preference, and ensure we use the same time zone as the APTRS.

```bash
ALTER ROLE aptrsdb_user SET client_encoding TO 'utf8';
ALTER ROLE aptrsdb_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE aptrsdb_user SET timezone TO 'UTC';

\c aptrsdb;
GRANT USAGE ON SCHEMA public TO aptrsdb_user;
GRANT ALL PRIVILEGES ON SCHEMA public TO aptrsdb_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO aptrsdb_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO aptrsdb_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON TABLES TO aptrsdb_user;
GRANT ALL PRIVILEGES ON DATABASE aptrsdb TO aptrsdb_user;
\q
```


After completing our tasks, we ensure that the PostgreSQL service is running and set to start automatically on reboot.

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```




## Redis Setup


To install Redis, run the command below.

```bash
sudo apt install redis-server
```

After installing Redis, we should ensure to change the `supervised` setting and add a password to our Redis service.

```bash
sudo nano /etc/redis/redis.conf
```
 
In the `redis.conf` file, add the following content. Ensure to update the password to a new, secure password.

```bash
supervised systemd
requirepass s!!D%AriPB-MO~5
```

After completing our tasks, we ensure that the Redis service is running and is set to start automatically on reboot.

```bash
sudo systemctl restart redis
sudo systemctl enable redis
sudo systemctl restart redis.service
```




## APTRS Django APIs Setup


Run the below command to install Python and other requirements for the APTRS.

```bash
sudo apt install python3-venv python3-dev libpq-dev weasyprint
```

It's better to separate the API and web service from other users on the machine. It's advisable to create a separate account for it.

```bash
sudo adduser aptrs
sudo usermod -aG sudo aptrs
usermod -a -G www-data aptrs
sudo su - aptrs
```

APTRS uses Python Poetry to manage dependencies, so we ensure its installation.

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

By default, Poetry is not added to the user or system environment. Ensure you add it to the environment.

```bash
export PATH="/home/aptrs/.local/bin:$PATH"
```

Once completed, we can download the APTRS from GitHub, install the dependencies, and copy the example .env file.

```bash
cd /home/aptrs
git clone https://github.com/APTRS/APTRS
cd APTRS
poetry install
cd APTRS
cp env.example .env  # The .env file should be located in the same directory as the manage.py file.


```
After obtaining the `.env` file from the demo environment file, ensure that you update it with the following details: the password for Redis, the PostgreSQL hostname or IP address, the Redis hostname or IP address, the secret key, and any other necessary information. It's important to review all the details in the `.env` file carefully. For more information on configuring the `.env` file and the various data it contains, please refer to the **[Environment Variables](/installation/env/)** section.

Few Important details for **.env** file:


- **WHITELIST_IP:** Add the server domain name if you're using one; if using a public IP, include that as well. For local network access, add the server's internal IP to the whitelist. Specify the protocol (HTTP or HTTPS) and use Nginx instead of the Django default port (8000). You don’t need to specify a port if using HTTP on 80 or HTTPS on 443. If using S3 buckets, include the bucket URL.

- **CORS_ORIGIN:** Since the frontend and backend will be on the same server, simply allow the server's public or local IP, along with the appropriate protocol and port, if necessary. Both frontend and backend should be accessible via the same domain or IP using Nginx reverse proxy. For more details, check the **[Frontend](/installation/frontend/)** documentation.

- **ALLOWED_HOST:** Set this only for the server's domain name or public/internal IP, based on how APTRS will be accessed.

- **SECRET_KEY:** Make sure to update it with a secure key; the same key will be used to generate a JWT token.


From the directory where manage.py is located, run the command below.

```bash
poetry shell
python3 manage.py makemigrations accounts
python3 manage.py makemigrations configapi
python3 manage.py makemigrations customers
python3 manage.py makemigrations project
python3 manage.py makemigrations vulnerability
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py first_setup
exit
```


Once we have completed the required setup, we need to start the server. APTRS uses Gunicorn for this purpose, which should already be installed through the steps we have completed so far.

### Setting up Gunicorn Service

First, we need to locate the full path of Gunicorn in our Poetry virtual environment:

```bash
$> cd /home/aptrs/APTRS
$> poetry run which gunicorn

## Example output (your path will likely be different):
/home/aptrs/.cache/pypoetry/virtualenvs/aptrs-h1P6HTQN-py3.12/bin/gunicorn
```

!!! tip "Important"
    Make note of this path as you'll need it in the next step when configuring the Gunicorn service.

Once we have the full path for Gunicorn, we can set up the `gunicorn.service` file:


```bash
sudo nano /etc/systemd/system/gunicorn.service
```

In the nano file editor, paste the following configuration. **Remember to replace the `ExecStart` path with the full Gunicorn path you found in the previous step**:

```bash
[Unit]
Description=gunicorn daemon to serve APTRS
Requires=gunicorn.socket
After=network.target

[Service]
User=aptrs
Group=www-data
WorkingDirectory=/home/aptrs/APTRS/APTRS
# Replace this path with your actual Gunicorn path from the previous step
ExecStart=/home/aptrs/.cache/pypoetry/virtualenvs/aptrs-h1P6HTQN-py3.12/bin/gunicorn --workers 3 --access-logfile - --bind unix:/run/gunicorn.sock APTRS.wsgi:application

[Install]
WantedBy=multi-user.target
```



Next, we need to set up the Gunicorn socket file:

```bash
sudo nano /etc/systemd/system/gunicorn.socket
```

Add the following configuration to the socket file:

```bash
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
```

### Starting and Enabling Gunicorn Services

Now that everything is configured, we can start and enable the Gunicorn services:

```bash
sudo systemctl start gunicorn.socket
sudo systemctl enable gunicorn.socket
sudo systemctl enable gunicorn
sudo systemctl daemon-reload
sudo systemctl restart gunicorn
```

### Verifying the API Service

To verify that the APTRS APIs are operational, run this command:

```bash
curl --unix-socket /run/gunicorn.sock localhost/api/config/ping/
```

You should receive a success response like this:
```json
{"status":"ok","message":"Server is up and running!"}
```

!!! tip "Troubleshooting"
    If you encounter any issues, check the status of the Gunicorn socket and service:
    ```bash
    sudo systemctl status gunicorn.socket
    sudo systemctl status gunicorn
    ```
    Look for any error messages in the output to help diagnose the problem.



## Frontend ViteJs Setup

This section covers the installation and configuration of the APTRS frontend built with Vite.js.

### Installing Node.js and NPM

First, install Node.js and NPM:

```bash
sudo apt install nodejs
sudo apt install npm
```

### Configuring the Frontend Environment

After installing NPM and Node.js, we need to create a `.env` file for the frontend that points to our backend API:

```bash
cd /home/aptrs/APTRS/frontend
cp env.example .env
nano .env
```

In the Nano editor, modify the content to the following:

```bash
VITE_APP_API_URL = /api/
VITE_APP_ENV = production
```

!!! note "API URL Configuration"
    We're using `/api/` as the backend URL since both the frontend and backend will be served from the same domain through Nginx's reverse proxy. For more details on frontend configuration, see the **[Frontend](/installation/frontend/)** documentation.

Now that we have configured the env file, we can install the required packages for the frontend with the command below:

```bash
npm install
```

Once we have all the packages installed, we can build the frontend using the command below.

```bash
npm run build
```

Once the build is completed, we can see all the front-end build at the directory below:

```bash
cd /home/aptrs/APTRS/frontend/dist
ls

android-chrome-192x192.png  assets             favicon.ico       logo.svg       stats.html
android-chrome-512x512.png  favicon-16x16.png  hero-desktop.png  manifest.json
apple-touch-icon.png        favicon-32x32.png  index.html        robots.txt

```


## Nginx Setup


We have everything set up, and our APIs are running with Gunicorn. Now, we need to configure the frontend to serve static files with Nginx and connect Gunicorn to Nginx. To get started, we first need to install Nginx using the command below:

```bash
sudo apt install nginx
```

Since our frontend and backend are in the user directory, we will encounter a permission error from Nginx. To resolve this, we will grant access to the www-data user group with the command below:

```bash
sudo chown -R aptrs:www-data /home/aptrs
```

To complete the setup, we can assume the domain name for our web server is `demo.aptrs.com`. The configuration below will have the nginx configuration files or folder names based on the domain, which users should replace with their actual domain.

Next, we will create a new server block in the Nginx sites-available directory using the following command:

```bash
sudo nano /etc/nginx/sites-available/demo.aptrs.com
```


Paste the Below nginx configuration,

```bash

# HTTP server configuration
server {
    listen 80;
    server_name demo.aptrs.com;  # Replace with your domain

    server_tokens off;
    client_max_body_size 100M;



    ### Host Validation - > Update according to your need
    if ( $host !~* ^(demo.aptrs.com)$ ) {
        return 444;
 }

    # Pass all /api/* to the Django backend
    location /api/ {
        proxy_pass http://unix:/run/gunicorn.sock;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $host;
        proxy_redirect off;
 }

    # Home page for frontend
    location / {
    root /home/aptrs/APTRS/frontend/dist;
    index index.html index.htm;
    try_files $uri $uri/ /index.html =404;
 }
    ## Server Static from Django over nginx
    location /static/ {
        alias /home/aptrs/APTRS/APTRS/static/;
 }

    # Blocked, accessing the whole media folder may lead to access to sensitive images like POC images,
    #location /media/ {
    #    alias /home/aptrs/APTRS/APTRS/media/;  # Path to your media files
    #}

    location = /favicon.ico {
    alias /home/aptrs/APTRS/frontend/dist/favicon.ico;
    access_log off;
    log_not_found off;
   
 }

    ## Server user profile photo
    location /media/profile/ {
        alias /home/aptrs/APTRS/APTRS/media/profile/;  # Path to your media files
 }

    ## Server Company Logo Images
    location /media/company/ {
        alias /home/aptrs/APTRS/APTRS/media/company/;  # Path to your media files
 }

    ## HTML Report design Images like background images
    location /media/report/ {
        alias /home/aptrs/APTRS/APTRS/media/report/;  # Path to your media files
 }

    access_log /var/log/nginx/APTRS_access.log;  # Path to access log file
    error_log /var/log/nginx/APTRS_error.log;   # Path to the error log file
}

```




Let’s enable the file by creating a link from it to the sites-enabled directory, which Nginx reads from during startup:

```bash
sudo ln -s /etc/nginx/sites-available/demo.aptrs.com /etc/nginx/sites-enabled/
```



Let's verify if any error from the nginx with

```bash
sudo nginx -t
```
If there are no errors we can start the nginx

```bash
sudo systemctl restart nginx
```

!!! success "Installation Complete"
    At this point, your APTRS installation should be complete and accessible via your domain or IP address. You can navigate to http://your-domain.com (or https:// if you've set up SSL) in your browser to access the APTRS interface.



### HTTPS Certificate

For production environments, it's highly recommended to secure your site with HTTPS. We can use Let's Encrypt and Certbot to obtain a free SSL certificate:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d demo.aptrs.com
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

The certbot utility will guide you through the process of obtaining and installing the certificate. It will also modify your Nginx configuration to use HTTPS and redirect HTTP traffic to HTTPS.

!!! tip "Auto-renewal"
    Certbot automatically adds a timer to renew certificates before they expire. You can verify this is working with the `sudo systemctl status certbot.timer` command.
