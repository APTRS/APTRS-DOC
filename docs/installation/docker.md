

## Linux

```bash
git clone https://github.com/APTRS/APTRS
cd APTRS
cp env.docker .env
nano .env
docker-compose up 

```

## Windows
```shell
git clone https://github.com/APTRS/APTRS
cd APTRS
copy env.docker .env
notepad .env
docker-compose up 
```



!!! note

    The **.env** file contains all the environment variables, such as passwords for JWT, Database credentials, Cloud S3 bucket details, etc. It’s important to update the default passwords and details before deploying the application. For more information, refer to the **[Environment Variables](/installation/env/)** section.

