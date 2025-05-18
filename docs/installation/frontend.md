# APTRS Frontend Configuration Guide

## Building the Frontend

APTRS includes the Vite.js frontend source code by default, giving you flexibility to customize the frontend as needed.

### Docker Deployment

If you're using Docker, the build process is automatic—no manual steps required.

### Manual Deployment

If you're manually deploying APTRS, you need to build the frontend yourself:

```bash
git clone https://github.com/APTRS/APTRS
cd APTRS
cd Frontend
cp env.example .env
npm run build
```

!!! note "Environment Configuration"
    The `.env` file contains critical settings including the backend API URL and environment type. The most important setting is `VITE_APP_API_URL`.

## Critical Requirement: Same-Domain Access

!!! danger "Critical Deployment Requirement"
    **APTRS requires that frontend and backend be accessible from the same domain, IP, and port.** While technically possible to configure differently, doing so will break core functionality.

### Why Same-Domain Access Is Mandatory

APTRS is designed with specific security and functionality requirements that make same-domain deployment necessary:

#### 1. Authentication System Limitations

- **Cookie-Based Authentication**: APTRS uses HTTP cookies alongside JWT tokens
- **Browser Security Restrictions**: Browsers implement Same-Origin Policy (SOP) that restricts cookie sharing between domains
- **Cross-Domain Challenges**: Authentication will fail or require complex workarounds when domains differ
- **Practical Impact**: Users may be repeatedly logged out or unable to access secured resources

#### 2. Image Storage and Display

- **How APTRS Stores Images**: When users upload images (especially in vulnerability reports), CKEditor stores the full URL
- **Hardcoded Paths**: URLs are saved with the domain name in the database
- **Domain Change Problems**: If your API domain changes, all previously saved images will break
- **Practical Impact**: Reports with missing evidence images look unprofessional to clients

#### 3. Browser Security Restrictions

- **Static Resource Limitations**: Browsers don't send authentication headers for images, CSS, etc.
- **Authentication Headers**: Can't be included in `<img>` tags and other static resource requests
- **Resource Access**: Protected images won't display without same-domain cookies
- **Practical Impact**: Vulnerability evidence, company logos, and other critical images won't display

### Real-World Example of Problems

When frontend and backend are on different domains:

```
Frontend: https://aptrs-app.com
Backend API: https://aptrs-api.com/api
```

1. User uploads an image through CKEditor for a vulnerability
2. Image is stored with URL `https://aptrs-api.com/api/media/vulnerability/evidence.jpg`
3. This full URL is saved in the database
4. If you later change the API domain to `https://new-api.com`:
   - All previously uploaded images break
   - Reports missing images look unprofessional
   - Vulnerability evidence is missing
   - You can't fix URLs already saved in the database

### The Solution: Same-Domain Deployment

When frontend and backend share the same domain:

```
Frontend: https://aptrs.com/
Backend API: https://aptrs.com/api/
```

1. User uploads an image through CKEditor
2. Image is stored with relative URL `/api/media/vulnerability/evidence.jpg`
3. This works regardless of domain changes
4. Authentication cookies work seamlessly
5. Reports always display correctly

## Deployment Options (While Maintaining Same-Domain Access)

### Option 1: Single Server Deployment

The simplest approach is hosting both frontend and backend on the same server:

1. Build the frontend as described above
2. Configure Nginx to serve:
   - Frontend from the `dist` directory
   - Backend by proxying requests to `/api/*` to your Django server

### Option 2: Separate Servers with Unified Domain

If you need separate servers for scalability:

1. Host frontend on Server A
2. Host backend on Server B
3. Configure a load balancer or reverse proxy (like Nginx) in front of both
4. Present a **single domain** to users
5. Route traffic based on path:
   - `/api/*` → Backend server
   - All other paths → Frontend server

```
╔═══════════════╗     ┌─────────────────┐
║               ║     │                 │
║  User Browser ║─────┤ Load Balancer   │
║               ║     │ (same domain)   │
╚═══════════════╝     └─────────────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
        ┌────────▼───────┐         ┌────────▼───────┐
        │                │         │                │
        │ Frontend       │         │ Backend        │
        │ Server         │         │ Server         │
        │                │         │                │
        └────────────────┘         └────────────────┘
```

## Configuration Steps

### Frontend `.env` Configuration

When properly set up with same-domain access, your frontend `.env` file should use:

```
VITE_APP_API_URL = /api/
VITE_APP_ENV = production
```

The crucial part is `/api/` (not a full URL), which ensures all API requests are relative to the current domain.

### Nginx Configuration Example

Here's a sample Nginx configuration for same-domain deployment:

```nginx
server {
    listen 80;
    server_name aptrs.yourdomain.com;

    # Frontend
    location / {
        root /path/to/aptrs/frontend/dist;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api/ {
        proxy_pass http://127.0.0.1:8000;  # Django backend
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # Media files
    location /media/ {
        proxy_pass http://127.0.0.1:8000;
    }

    # Static files
    location /static/ {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

## Troubleshooting

If you experience issues with images not displaying or authentication problems, check:

1. That frontend and backend share the same domain
2. Your frontend `.env` file uses just `/api/` (not a full URL)
3. Nginx or your proxy is correctly routing `/api/*` paths to the backend
4. CORS and cookie settings in your Django backend configuration

!!! warning "Never Configure Separate Domains"
    While technically possible to configure, using separate domains for frontend and API will cause persistent, difficult-to-fix problems with authentication and image display that significantly impact the usability of APTRS.