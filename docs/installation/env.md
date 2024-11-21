The APTRS backend uses the ``.env`` file to store credentials such as S3 bucket information, database credentials, secret keys, whitelisted IPs/domains, and more.

- If you are deploying the application with Docker, make sure to update the details in the .env file from the project root.
- If you are deploying the application without Docker, you will need to update specific details like the S3 bucket and whitelisted IPs.

| ENV         | Description                          | Docker |     Linux Server Setup
| ------------------| ----------------------------- | ------------------------------------ | ------------------------------------ |
| `SECRET_KEY`       | This key is used by Backend including JWT, Should be secured and random.  | Manually need to be updated in env file| Auto Generated no need to update it. |
| `WHITELIST_IP`       | This allows to set whitelisted IP/domain with port number to allow loading resource during PDF report to prevent SSRF vulnerability. | Manually need to be updated in env file | Auto Generated but Manually need to be updated in env file based on your domain name, IP etc. |
| `ALLOWED_HOST `    | Whitelist allowed host to prevent host header injection attack | Manually need to be updated in env file | Auto Generated but Manually need to be updated in env file based on your domain name, IP etc. |
|`CORS_ORIGIN ` | Whitelist allowed origin to prevent cross origin attack | Manually need to be updated in env file | Auto Generated but Manually need to be updated in env file based on your domain name, IP etc. |
|`REDIS_URL` | Redis Server Details including IP, Port and password | Should replace the default password `q8N8HwlaOWqOl1hG7rdmBsm7oT52fLKHZXFwOB4VM7SXFDV8wg` to a new strong random password. | Auto Generated no need to update it. |
|`REDIS_PASSWORD` | Redis Server password for Redis image in docker | Should replace the default password `q8N8HwlaOWqOl1hG7rdmBsm7oT52fLKHZXFwOB4VM7SXFDV8wg` to a new strong random password. Password in **REDIS_URL**  and **REDIS_PASSWORD** should be same| Not needed and no need to update/add. |
|`POSTGRES_USER` , `POSTGRES_PASSWORD`, `POSTGRES_PORT`, `POSTGRES_DB` | Postgres DB username, password, port, and DB names | Manually need to be updated in env file | Auto Generated no need to update it. |
|`POSTGRES_HOST` | Postgres host name | Should not be updated. | Auto Generated no need to update it. |
|`USE_S3`| If you want to use Cloud S3 bucket Digital Ocean or AWS s3 bucket. Default ``False``, you can change it to `True`|Optional| Optional |
| `AWS_ACCESS_KEY_ID` `AWS_SECRET_ACCESS_KEY` `AWS_STORAGE_BUCKET_NAME` `AWS_S3_REGION_NAME` `AWS_S3_CUSTOM_DOMAIN` `AWS_S3_ENDPOINT_URL` | Bucket details if `USE_S3` is set to `True`|Optional| Optional|