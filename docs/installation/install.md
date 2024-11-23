APTRS can be installed using two methods:

- Docker (Recommended for most users)
- Manual (User with Knowledge of Nginx and Little Python and Nodejs)


!!! note

    APTRS does not natively support Windows installations and setting it up on Windows is not recommended. While it's possible to manually install certain components (such as the APTRS Backend, Frontend, and Database) on Windows, key dependencies like Redis require a Linux environment. Instead, you can use the Docker setup for Windows, which is the preferred option for Windows users.



!!! note

    Default creds are sourav.kalal@aptrs.com & I-am-Weak-Password-Please-Change-Me



!!! note

    APTRS uses cookie attributes such as Secure, HttpOnly, and Lax. For users with a Docker setup, these attributes will not have any effect. However, users with a manual setup need to ensure that they use HTTPS and that both the front end and back end are on the same domain or IP address.