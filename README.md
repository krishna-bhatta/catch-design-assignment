# Project Setup Guide

### Ensure No Port Conflicts

Before starting the services, ensure that no other services are using the following ports:

- **Port 80**: Nginx (Frontend/Backend)
- **Port 8080**: PHPMyAdmin
- **Port 3001**: Frontend (React)

If any of these ports are in use by other services, you may need to stop those services or change the ports in this configuration.

### Prerequisites

Make sure you have the following installed:

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Install Docker Compose](https://docs.docker.com/compose/install/)

### Project Setup

1. **Clone the repositories**:

   First, clone the main project, frontend, and backend repositories:

   - Main Project Repository (contains Docker configuration):
     ```bash
     git clone https://github.com/krishna-bhatta/catch-design-assignment.git
     cd catch-design-assignment
     ```

   - Frontend Repository:
     ```bash
     git clone https://github.com/krishna-bhatta/cda-frontend.git frontend
     ```

   - Backend Repository:
     ```bash
     git clone https://github.com/krishna-bhatta/cda-backend.git backend
     ```

2. **Configure `.env` for Frontend**:

   In the `frontend` folder, copy the example environment file and configure the API base URL:

   ```bash
   cp .env.example .env
   ```

   Edit the `.env` file and set the API base URL to the backend endpoint:

   ```env
   REACT_APP_API_BASE_URL=http://localhost/api
   ```

3. **Configure `.env` for Backend**:

   In  `./backend/.env` file, configure the following environment variables based on the service configurations:

   - **Database Configuration**:
     ```env
     DB_CONNECTION=mysql
     DB_HOST=cda-mysql
     DB_PORT=3306
     DB_DATABASE=cda_db
     DB_USERNAME=root
     DB_PASSWORD=rootpassword
     ```

   - **Frontend URL** (for frontend to fetch data from the backend):
     ```env
     FRONTEND_URL=http://localhost:3001
     ```

4. **Folder Structure**

   Ensure  folder structure looks like this:

   ```bash
   catch-design-assignment/
   ├── backend/                  #  backend code
   ├── frontend/                 #  frontend code
   ├── docker/                   # Docker configuration files
   │   ├── nginx/                # Nginx configuration files
   │   ├── php/                  # PHP configuration files
   │   └── mysql/                # MySQL configuration files
   └── docker-compose.yml        # Docker Compose configuration file
   ```

   Make sure all the necessary directories are in place:

   - **`./backend/`**: Contains  backend code (Laravel).
   - **`./frontend/`**: Contains  frontend code (React).
   - **`./docker/nginx/`**: Contains the Nginx configuration files (e.g., `nginx.conf`).
   - **`./docker/php/`**: Contains PHP configuration files (e.g., `php.ini`).
   - **`./docker/mysql/`**: Contains MySQL configuration files (e.g., `my.cnf`).

   If any of these directories are missing, you’ll need to create them manually or clone the corresponding repositories to populate the `backend` and `frontend` folders.

5. **Build and start the services**:

    After setting up the environment and folder structure, you can run the following command to build and start the services:
   from inside main project `catch-design-assignment`
   
    ```docker-compose build```
   ```docker-compose up -d``` -> If you encounter with any issue check port or see log(see Troubleshooting)
7. **Install Dependencies for Backend**:

    For the backend (Laravel), install the necessary dependencies and run migrations:

    ```bash
    docker exec -it cda-web composer install
    docker exec -it cda-web php artisan key:generate
    docker exec -it cda-web php artisan migrate
    docker exec -it cda-web php artisan db:seed --class=CustomerSeeder
    ```
8. **Access the Services**:
- Frontend: http://localhost:3001
- Backend API: http://localhost/api
- PHPMyAdmin: http://localhost:8080
- MySQL: 
    - Port 3306
    - username: root
    - password: rootpassword

8. **Customization**
- Modify the `./docker/nginx/nginx.conf` to customize your Nginx configuration.
- Customize `./docker/mysql/my.cnf` to configure - MySQL as per your requirements.
- Modify the `./docker/php/php.ini` file to tweak PHP settings.

9. **Troubleshooting**
- Container not starting: Check logs by running docker-compose logs <service-name>.
- File permission errors: Make sure your backend and frontend files have the correct permissions, especially if running on Linux or macOS.
