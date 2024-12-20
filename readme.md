# Docker Compose with Nginx, Certbot, and SSL Redirections

This project uses **Docker Compose** to orchestrate a backend, a frontend, a database, and an **Nginx** server. Nginx handles HTTPS redirections using certificates generated by **Certbot** or **Openssl**.

## Prerequisites

Before starting, make sure you have installed the following tools:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- **Certbot** for managing SSL certificates.
- **Openssl** for generating self-signed certificates.

---

## Features

1. **Backend**: A Node.js server configured using environment variables.
2. **Database**: MariaDB (or whatever you want) with an initial schema provided via `schema.sql`.
3. **Frontend**: A React application.
4. **Nginx**: Acts as a reverse proxy, routing traffic and handling HTTPS on your vps.

---

## Installation and Setup

### Step 1: Environment Configuration

You can use separate environment files for different environments. Create `.env.dev` for development and `.env.prod` for production. Example content for `.env.dev`:

### Example `.env.dev` file
```env
#Backend
DATABASE_DB=your_database_name
DATABASE_USER=your_database_user
DATABASE_PASSWORD=your_database_password
DATABASE_HOST=db
NODE_ENV=development
ACCESS_TOKEN_SECRET=your_access_token_secret
REFRESH_TOKEN_SECRET=your_refresh_token_secret
CLIENT_URL=http://localhost:3000
PORT=8080

#Frontend
REACT_APP_API_URL=http://localhost:8080
```

### Example `.env.prod` file
```env
#Backend
DATABASE_DB=your_database_name
DATABASE_USER=your_database_user
DATABASE_PASSWORD=your_database_password
DATABASE_HOST=db
NODE_ENV=production
ACCESS_TOKEN_SECRET=your_access_token_secret
REFRESH_TOKEN_SECRET=your_refresh_token_secret
CLIENT_URL=https://your_domain_name
PORT=8080

#Frontend
REACT_APP_API_URL=https://your_domain_name
```

### Step 2: Install Certbot or Openssl

You can install **Certbot** using the following command:

```bash
sudo apt install certbot python3-certbot-nginx
```

```bash
sudo apt install openssl
```

### Step 3: Generate SSL Certificates

You can generate SSL certificates using **Certbot** or **Openssl**. Here is an example using **Openssl**:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

And an example using **Certbot**:

```bash
sudo certbot certonly --nginx -d your_domain_name
```

after that you can modifie the `default` file to use the certificates generated by **Openssl** or **Certbot** in the
following path in debian machine.

```bash
sudo nano /etc/nginx/sites-available/default
```

### Step 4: Lauch the Docker Compose

You can start the services using the following command for development:

```bash
docker-compose --env-file .env.dev up --build
```

and for production:

```bash
docker-compose --env-file .env.prod up --build
```

### Conclusion

You can see the frontend application running on `http://localhost:3000` and the backend on `http://localhost:8080`. The Nginx server will handle HTTPS redirections and serve the frontend application.
Feel free to modify the configuration files to suit your needs.
My configuration files are available in the `openssl` and `certifBot` folder and docker-compose.yml are available at the root of the repository.
