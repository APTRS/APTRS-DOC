APTRS can be installed on DigitalOcean using a Droplet. To proceed with the installation, follow the **[Manual Installation Guide](/installation/manual)**.

In the manual installation process, each service (e.g., Redis, PostgreSQL, etc.) is set up as unmanaged, meaning you manually install and configure everything. Alternatively, you can leverage DigitalOcean's Managed Services, such as Redis and PostgreSQL. For storage, you can use Spaces Object Storage.


### Getting Started

To get started, you need to set up the following:

- **PostgreSQL Managed Database**: For storing application data.
- **Redis Managed Database**: For caching and session management.
- **Droplet**: To host the frontend and backend code.
- **Spaces Object Storage**: For storing images and uploaded data.

### PostgreSQL Setup

Follow these steps to set up a PostgreSQL Managed Database in DigitalOcean:

1. Navigate to [DigitalOcean Databases](https://cloud.digitalocean.com/databases) and select **Create Database Cluster**.
2. Choose the desired **Database Region**.
3. From the list of database engines, select **PostgreSQL v14**.
4. Configure the **CPU, database resources, and storage** according to your requirements.
5. Add a **Cluster Name**, or leave it as the default.
6. Click **Create Database** to finalize the setup.

7. Navigate to the newly created **PostgreSQL** database.
8. In the **Users and Databases** section, create a new **database** and **user** for APTRS.



### Redis Setup

1. Navigate to [DigitalOcean Databases](https://cloud.digitalocean.com/databases) and select **Create Database Cluster**.
2. Choose the desired **Database Region**.
3. From the list of database engines, select **Caching**.


4. Configure the **CPU, database resources, and storage** according to your requirements.
5. Add a **Cluster Name**, or leave it as the default.
6. Click **Create Database** to finalize the setup.


### Space Object Storage Setup

1. Navigate to [DigitalOcean Spaces](https://cloud.digitalocean.com/spaces?i=aefe60) and select **Create a Space Bucket**.
2. Choose the **Region** and add a **Bucket Name**.



3. Click **Create Space Bucket** to complete the setup.
4. Navigate to [Spaces Access Keys](https://cloud.digitalocean.com/spaces/access_keys).
5. Create a new **Access Key** with **Read/Write/Delete** permissions.
6. Copy the **Access Key** and **Secret Key** for later use.


### Droplet

1. Now to deploy APIs and Frontend we will use Droplet
2. Navigate to https://cloud.digitalocean.com/droplets/new?i=aefe60&region=blr1&size=s-2vcpu-2gb and create a new Ubutnu Droplet
3. SSH into the newly created droplet with root user

Run the below Commands into the droplet

```bash
sudo apt update
sudo apt install python3-venv python3-dev libpq-dev weasyprint postgresql-client-common
sudo adduser aptrs
sudo usermod -aG sudo aptrs
usermod -a -G www-data aptrs
sudo su - aptrs
curl -sSL https://install.python-poetry.org | python3 -
export PATH="/home/aptrs/.local/bin:$PATH"
cd /home/aptrs
git clone https://github.com/APTRS/APTRS
cd APTRS
poetry install
cd APTRS
cp env.example .env
```


In the `.env` file, add the following details:
- **Bucket Details**: Access Key, Secret Key, and Bucket Name.
- **Database Details**: Connection strings for PostgreSQL and Redis.
- **Domain Name**: Your application's domain.

You can refer to the sample `.env` file below for guidance.
```python
SECRET_KEY='3-3hnf1kkn#x0350(we+9^m@69xc3_e_@_7$2tf=d)6$i*t_0#'  # change secret key 
WHITELIST_IP= ["http://demo.aptrs.com"]   ## your domain name
ALLOWED_HOST = ["demo.aptrs.com"]    ## your domain/host name without http https
CORS_ORIGIN = ["https://demo.aptrs.com"]  ## CORS domain keep your domain 
REDIS_URL='redis://<username>:<password>@<host>:<port>/'
POSTGRES_USER="<user>"
POSTGRES_PASSWORD="<password>"
POSTGRES_HOST="<host>"
POSTGRES_PORT=25060
POSTGRES_DB="<DB Name>"
USE_DOCKER=False
USER_TIME_ZONE="Asia/Kolkata"
USE_S3=True
AWS_ACCESS_KEY_ID='<Bucket Access KEY>'
AWS_SECRET_ACCESS_KEY='<Bucket Secret KEY>'
AWS_STORAGE_BUCKET_NAME='<BUCKET NAME>'
AWS_S3_REGION_NAME='nyc3' #keep it nyc3 unless error in connection
AWS_S3_CUSTOM_DOMAIN='<BUCKET URL>/'  ## ex https://aptrs.blr1.digitaloceanspaces.com/  - Make sure ends with /
AWS_S3_ENDPOINT_URL='<BUCKET URL>/'  ## ex https://aptrs.blr1.digitaloceanspaces.com/  - Make sure ends with /

```


Once the env file is setup, run the below command

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


Once we have completed the required setup, we need to start the server. APTRS uses Gunicorn for this purpose, which should already be installed through the steps we have completed so far. Since we are using Poetry, we need to locate the full path of Gunicorn for the APTRS project using the command below:

```bash
$> cd /home/aptrs/APTRS
$> poetry run which gunicorn

##We might get the full path like below, this will be different for all users
/home/aptrs/.cache/pypoetry/virtualenvs/aptrs-h1P6HTQN-py3.12/bin/gunicorn
```

Once we have the full path for gunicorn, we can set up the `gunicorn.service` with the command below:


```bash
sudo nano /etc/systemd/system/gunicorn.service
```

In the nano file editor, paste the following text. Ensure to update the full path for gunicorn in the `gunicorn.service` file.



```bash
[Unit]
Description=gunicorn daemon to serve APTRS
Requires=gunicorn.socket
After=network.target

[Service]
User=aptrs
Group=www-data
WorkingDirectory=/home/aptrs/APTRS/APTRS
ExecStart=/home/aptrs/.cache/pypoetry/virtualenvs/aptrs-h1P6HTQN-py3.12/bin/gunicorn --workers 3 --access-logfile - --bind unix:/run/gunicorn.sock APTRS.wsgi:application

[Install]
WantedBy=multi-user.target
```



We can now set up the `gunicorn.socket` using the command below:

```bash
sudo nano /etc/systemd/system/gunicorn.socket
```

```bash
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target

```

Now that everything is ready, we can start the Gunicorn service using the command below.

```bash
sudo systemctl start gunicorn.socket
sudo systemctl enable gunicorn.socket
sudo systemctl enable gunicorn
sudo systemctl daemon-reload
sudo systemctl restart gunicorn
```

You can verify whether the APTRS APIs are operational by using the command below.

```bash
curl --unix-socket /run/gunicorn.sock localhost/api/config/ping/

{"status":"ok","message":"Server is up and running!"}
```

In case of an error, you can check if the socket and service have any issues using the command below.
```bash
sudo systemctl status gunicorn.socket
sudo systemctl status gunicorn
```



## Frontend ViteJs Setup

To install Node.js, run the command below.

```bash
sudo apt install nodejs
sudo apt install npm
```


After installing NPM and Node.js, we need to create a `.env` file for the front end. This `.env` file will contain the URL for the backend Django API. Since we plan to deploy both the API and the frontend on the same server and they will be accessible through the same domain or IP address using Nginx, we will specify the backend URL as `/api/`. For more details, please refer to the **[Frontend](/installation/frontend/)** documentation.


```bash
cd /home/aptrs/APTRS/frontend
cp env.example .env
nano .env
```

In the Nano editor, modify the content to reflect the following changes:

```bash
VITE_APP_API_URL = /api/
VITE_APP_ENV = production
```

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




### HTTPS Certificate

Once we have the domain name we can get the CA certificate with certbot using the below command. 


```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d demo.aptrs.com
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```