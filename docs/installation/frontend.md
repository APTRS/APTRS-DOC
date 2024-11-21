APTRS now includes the source code of the Vite.js frontend by default, instead of a pre-built version. This change allows you greater flexibility to customize the frontend as needed. If you are deploying the application using Docker, the build process is handled automatically, so you don't need to worry about creating a production build manually.

However, if you choose to manually deploy APTRS, you will need to manually build the production version of the frontend and serve it using Nginx or any other reverse proxy.

To build the production version from the Vite.js source code, follow these steps:




``` bash
git clone https://github.com/APTRS/APTRS
cd APTRS
cd Frontend
cp env.example .env
npm run build
```

!!! note

    The **.env** file contains settings like the backend API domain and app environment. You can modify these as needed.


By default, APTRS serves both the backend and frontend on the same server, meaning they share the same IP/domain and port. The ``.env`` file uses just ``/api/`` in docker build for the backend API domain. This setup is recommended to avoid issues with image uploads in the CKEditor within APTRS and to prevent conflicts related to Same-Origin policies.

<hr style="border: none; height: 2px; background-color: #000; margin: 20px 0;">>





### Hosting the Frontend and API Together

If you wish to host the frontend on a separate domain, IP, or port from the backend API, you can configure the backend API's domain in the .env file within the Frontend folder. For example:


```bash
nano .env

VITE_APP_API_URL = http://backend-api.com/api/
npm run build
```

However, it is not recommended to host the API and frontend on separate domains, IPs, or ports. Here's why:

1. JWT Token Authentication and Cookies:

    - The API uses a combination of JWT tokens in headers and cookies for authentication and access control.
    - Cookies are domain-specific and cannot be shared across different domains unless explicitly configured with a domain-wide scope. This can lead to complexities in maintaining secure cross-domain authentication.

2. Static Image Loading Issues:

    - Browsers do not allow custom headers, such as JWT tokens, for loading static resources like images.
    - APTRS addresses this by using cookies for authentication, allowing images protected by the API to be loaded by authenticated users. However, this setup is difficult to manage across separate domains, as it relies on cookies being available in the same domain scope.

4. Unified Image Path Management:

    - APTRS supports storing images locally or on S3. Using the API for image access ensures consistency, as images are accessed through the same API endpoint regardless of their storage location.
    - This approach prevents exposing hardcoded S3 bucket URLs in CKEditor or frontend code. Additionally, if the S3 bucket URL changes or signed URLs expire, the API path remains consistent.

4. Image Management with CKEditor:

    - APTRS manages images uploaded through CKEditor by serving them directly via the backend API, rather than using a web server. This design ensures that sensitive images, such as those used for proofs of concept (POCs), are accessible only to authenticated users. These images are protected by authentication tokens and are not publicly exposed.
    - CKEditor uses ``<img>`` tags with a ``src`` attribute pointing to the API's image path. For example:

        ```html
        <img src="https://api.yourdomain.com/api/path/to/image.jpg" />
        ```

### Challenges with Hosting the API and Frontend on Separate Domains

When the API is hosted on a separate domain from the frontend:

- CKEditor hardcodes the API domain in the ``src`` attribute of the images.
- If the API domain name, IP, or port changes, previously uploaded images will fail to load, as CKEditor cannot dynamically update the ``src`` attribute.
- Additionally, browsers loading static resources like images do not support custom headers (e.g., JWT tokens), which can complicate access control for images requiring authentication.

### Benefits of Hosting API and Frontend on the Same Domain

If both the API and frontend are hosted on the same domain, the frontend can be built with ``VITE_API_URL=/api/``. This approach allows CKEditor to generate image paths relative to the frontend's base domain, such as:

```html
     <img src="/api/path/to/image.jpg" />
```

This configuration provides several advantages:

- Domain Independence: The image path (``/api/path/to/image.jpg``) remains valid even if the domain name, IP address, or port changes, as long as the API and frontend share the same base domain.
- Simplified Image Access: The relative path ensures that images are seamlessly accessible without hardcoding the API domain, reducing the risk of broken links if deployment configurations change.
- Consistent User Experience: End users can load images securely without encountering authentication or cross-domain issues.



### Recommendation
For best results:

1. Host Both API and Frontend on the Same Domain

    - Use a reverse proxy, such as Nginx, to host both the API and frontend on the same domain.
    - Configure the frontend to use VITE_API_URL=/api/.

    This setup improves maintainability and ensures robustness against deployment changes, as the relative paths remain valid even if the domain name, IP, or port changes.

2. Deploying Frontend and Backend on Separate Servers

    - If you require separate servers for the frontend and backend (e.g., for scalability, load balancing, or easier maintenance), you can still serve both on the same domain or IP and port for users by using a reverse proxy.

    - For example, you can deploy the frontend on one server and the backend on another, but configure your reverse proxy to route requests based on the path:

        - Requests to ``/api/*, media/*, static/*`` are proxied to the backend server.
        - Other requests (e.g., for the frontend) are served by the frontend server.