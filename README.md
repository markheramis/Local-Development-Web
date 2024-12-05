# Local Development Web

## Overview

The Local Development Web project is designed to facilitate local development by providing a comprehensive setup that includes Traefik for domain management and SSL. Simulating domain and SSL locally is important because it allows developers to test and debug their applications in an environment that closely mirrors production. Additionally, the project includes a database setup with MySQL and Redis, and various dockerized submodules.

### Key Features:
- **Traefik Integration**: Traefik is used to manage domains and SSL certificates, making it easier to work with multiple services locally.
- **Database Services**: The project includes a database setup located in the `database/` directory, which contains MySQL and Redis services.
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

2. Copy the example environment file and update it with your configuration:
   ```sh
   cp .env.example .env
   ```

3. Create the `web` and `database` networks if they don't exist:
   ```sh
   docker network create web
   docker network create database
   ```
   **Note:** These networks are used to connect the services together.
   - `web` will be the network that we'll be using for exposing http services to the host machine.
   - `database` will be the network that we'll be using for connecting to the database service.

4. Install self-signed certificates:

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

5. Start Traefik:
   ```sh
   (cd traefik && docker compose up -d)
   ```
   This command will start traefik and expose its dashboard at port 8080.

5. Start Database:

   First you need to copy the `.env.example` file to `.env` and set the correct values for the environment variables.
   ```sh
   (cd database && cp .env.example .env)
   ```

   Make sure to inspect the `.env` file and make corrections as needed.

   Then you can start the database services:

   ```sh
   (cd database && docker compose up -d)
   ```
   This command will start the database services such as MySQL and Redis; it also exposes a web interface for managing the MySQL database at [db.docker.localhost](https://db.docker.localhost).

## Contributing

We welcome contributions to improve this project. Please fork the repository and submit a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contact

For any questions or support, please contact the project maintainers.

Happy coding!