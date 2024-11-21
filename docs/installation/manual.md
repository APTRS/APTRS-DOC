# Manual Installation

If you prefer to install the APTRS without docker and have more control over each serive like Redis, Postgresql, Ngnix etc, Or you want to host Data base on diffrent server, redis on diffrent etc. You can follow the below process on deplying the APTRS on a server without doceker, The steps mentieod below were testing on below setup. 


- OS - Ubutnu 24.10
- Python - 3.12.7  (3.9+ is required)
- Python Poetry - 1.8.4
- PostgreSQL - 16.4 (Ubuntu 16.4-1build1)
- Redis Server - 7.0.15
- Nginx - 1.26.0

## PostgreSQL Setup 

Run the below command to install PostgreSQL.

```bash
sudo apt-get install  postgresql postgresql-contrib
```

Once the installation is completed we can access the PostgreSQL shell with below command.

```bash
sudo -u postgres psql
```

From the PostgreSQL shell Create new database for APTRS project (Change the name you want recocmend to keep relenvet name to the project)

```bash
CREATE DATABASE aptrsdb;
```

Next, create a database user for our project. Make sure to select a secure password:

```bash
CREATE USER aptrsdb_user WITH PASSWORD 's!!D%AriPB-MO~5';
```

We are setting the default encoding to UTF-8 and change other settings. Make sure to update the Time zone as per your choise, also we need to make sure we use the same timezone the APTRS as well.

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


Once we are done, we make sure the postgresql service is running and always up with reboot.

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```




## Redis Setup


Run the below command to install Redis.

```bash
sudo apt install redis-server
```

Once redis is installed, we want to make sure we change the `supervised` and add password to our redis service.

```bash
sudo nano /etc/redis/redis.conf
```
 
Within `redis.conf` file add the below contenxt, Make sure to update the password to a new secure password

```bash
supervised systemd
requirepass s!!D%AriPB-MO~5
```

Once we are done, we make sure the redis service is running and always up with reboot.

```bash
sudo systemctl restart redis.service
sudo systemctl restart redis
sudo systemctl enable redis
```




## APTRS Django APIs Setup


Run the below command to install Python and other requirements for the APTRS.

```bash
sudo apt install python3-venv python3-dev libpq-dev weasyprint
```

Its better to seperate the API and webserice from other users in the machine, Its good to create a seperate account for it.

```bash
sudo adduser aptrs
sudo usermod -aG sudo aptrs
sudo su - aptrs
```

APTRS make use of Python Poetry to manage the dependices we make sure we install it.

```bash
curl -sSL https://install.python-poetry.org | python3 -
``` 

By default Poetry will not be added into the user or system enviment. Make sure to add it to the enviment.

```bash
export PATH="/home/aptrs/.local/bin:$PATH"
```

Once done we can download the APTRS from the GitHub, install the dependicies, and make copy the example .env file.

```bash
cd /home/aptrs
git clone https://github.com/APTRS/APTRS
cd APTRS
poetry install
cp env.docker APTRS/.env  # The .env file should be located in the same directory as the manage.py file.
cd APTRS

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


Once we have completed the required setup, we need to start the server. APTRS uses Gunicorn for this purpose, which should already be installed through the steps we have completed so far. Since we are using Poetry, we need to locate the full path of Gunicorn for the APTRS project using the command below:

```bash
$> cd /home/aptrs/APTRS
$> poetry run which gunicorn

##We might get the full path like below, this will be different for all users
/home/aptrs/.cache/pypoetry/virtualenvs/aptrs-h1P6HTQN-py3.12/bin/gunicorn
```

Once we have full path for gunicorn we can setup the `gunicorn.service` with below command:


```bash
sudo nano /etc/systemd/system/gunicorn.service
```

In the nano file editor paste the below context. Make sure to update the full path of the gunicorn in the below file for `gunicorn.socket`.



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



Now we can setup the `gunicorn.socket` with below command:

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

With all set We can start the gunicorn service with below command

```bash
sudo systemctl start gunicorn.socket
sudo systemctl enable gunicorn.socket
sudo systemctl enable gunicorn
sudo systemctl daemon-reload
sudo systemctl restart gunicorn
```

You can verify if APTRS APIs are up and running with below command

```bash
curl --unix-socket /run/gunicorn.sock localhost/api/config/ping/

{"status":"ok","message":"Server is up and running!"}
```

In case some error you can check if the socket and service has any error or not with below command
```bash
sudo systemctl status gunicorn.socket
sudo systemctl status gunicorn
```



### Frontend ViteJs Setup

## Nginx Setup