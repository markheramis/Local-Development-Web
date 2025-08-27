# Local Development Web

## Overview

The Local Development Web project is designed to facilitate local development by providing a comprehensive setup that includes Traefik for domain management and SSL. Simulating domain and SSL locally is important because it allows developers to test and debug their applications in an environment that closely mirrors production. Additionally, the project includes a modular database setup with MySQL, PostgreSQL, and Redis, MinIO object storage, and various dockerized submodules.

### Key Features:
- **Traefik Integration**: Traefik is used to manage domains and SSL certificates, making it easier to work with multiple services locally.
- **Database Services**: The project includes a modular database setup located in the `database/` directory, which contains MySQL, PostgreSQL, and Redis services.
- **MinIO Integration**: Object storage service compatible with Amazon S3 API, accessible via web console for development.
- **Project Directory**: The project includes a `project/` directory with a `.gitignore` rule to ignore everything there, making it reusable for any other purposes. This allows for flexibility and customization based on specific project needs.
- **Dynamic Design**: The project is designed to be dynamic, accommodating various services and configurations. This flexibility ensures that new services can be added and managed with ease.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose V2](https://docs.docker.com/compose/install/)
- [mkcert](https://github.com/FiloSottile/mkcert)

## Getting Started

1. Clone the repository:
   ```sh
   git clone https://github.com/markheramis/local-web.git
   cd local-web
   ```

2. Create the `web` and `database` networks if they don't exist:
   ```sh
   docker network create web
   docker network create database
   ```
   **Note:** These networks are used to connect the services together.
   - `web` will be the network that we'll be using for exposing http services to the host machine.
   - `database` will be the network that we'll be using for connecting to the database service.

3. Install self-signed certificates:

   ```sh
   mkcert -install
   ```

   ```sh
   mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "docker.localhost" "*.docker.localhost" "example.com" "*.example.com"
   ```
   > The `mkcert` command will create the certificates and place them in the `certs/` directory.

   > The `mkcert` command will also add the certificates to the local machine's trust store, so you don't need to do anything else.

   > The `mkcert` command will create the certificates for the domains `docker.localhost` and `example.com`, and all of their subdomains.

   > The `example.com` domain can be replaced with any other domain that you want to use however the `docker.localhost` domain is required for the docker network to work.

4. Start Traefik:
   ```sh
   (cd traefik && docker compose up -d)
   ```
   This command will start traefik and expose its dashboard at [traefik.docker.localhost](https://traefik.docker.localhost).

5. Start Database Services:

   The database services are modular and can be started individually based on your needs:

   **MySQL with phpMyAdmin:**
   ```sh
   (cd database/mysql && docker compose up -d)
   ```
   This starts MySQL and exposes phpMyAdmin at [db.docker.localhost](https://db.docker.localhost).

   **PostgreSQL:**
   ```sh
   (cd database/pgsql && docker compose up -d)
   ```

   **Redis:**
   ```sh
   (cd database/redis && docker compose up -d)
   ```

   **Start all database services:**
   ```sh
   (cd database/mysql && docker compose up -d)
   (cd database/pgsql && docker compose up -d)  
   (cd database/redis && docker compose up -d)
   ```

   Each database service uses environment variables that can be customized by creating `.env` files in their respective directories.

6. Start MinIO (Optional):

   MinIO provides S3-compatible object storage for development:

   ```sh
   (cd minio && docker compose up -d)
   ```

   After starting, MinIO will be available at:
   - **API**: [minio-api.docker.localhost](https://minio-api.docker.localhost) (for S3-compatible API calls)
   - **Console**: [minio-console.docker.localhost](https://minio-console.docker.localhost) (web interface)

   Default credentials: `minioadmin` / `minioadmin`

## Example

To run the example simply go to examples and run the compose project:

```sh
cd examples/whoami
docker compose up -d
```

the whoami service is configured to run in the domain [whoami.docker.localhost](whoami.docker.localhost), so if you navigate your browser to that URL, it should show you whoami logs.

## Contributing

We welcome contributions to improve this project. Please fork the repository and submit a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contact

For any questions or support, please contact the project maintainers.

Happy coding!
