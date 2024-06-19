# Django on Docker with Postgres, Gunicorn, and Nginx

This repository contains a Django application containerized using Docker. The setup includes PostgreSQL as the database, Gunicorn as the application server, and Nginx as a reverse proxy.

## Features

- **Django 4.2.3**: The web framework.
- **PostgreSQL**: The database.
- **Docker**: Containerization.
- **Gunicorn**: WSGI HTTP server for serving the Django application.
- **Nginx**: Reverse proxy and static file handler.

## Prerequisites

- Docker
- Docker Compose

## Project Structure

```plaintext
.
├── .env.dev
├── .env.prod
├── .env.prod.db
├── .gitignore
├── app
│   ├── Dockerfile
│   ├── Dockerfile.prod
│   ├── entrypoint.prod.sh
│   ├── entrypoint.sh
│   ├── hello_django
│   │   ├── __init__.py
│   │   ├── asgi.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── manage.py
│   └── requirements.txt
├── docker-compose.prod.yml
├── docker-compose.yml
└── nginx
    ├── Dockerfile
    └── nginx.conf
```

## Getting Started

### Setup Development Environment

1. **Clone the repository:**

    ```bash
    git clone https://github.com/BAXTOR95/django-on-docker.git
    cd django-on-docker
    ```

2. **Create environment variables:**

    Create a `.env.dev` file in the project root and add the following:

    ```plaintext
    DEBUG=1
    SECRET_KEY=your_secret_key
    DJANGO_ALLOWED_HOSTS=localhost 127.0.0.1 [::1]
    SQL_ENGINE=django.db.backends.postgresql
    SQL_DATABASE=hello_django_dev
    SQL_USER=hello_django
    SQL_PASSWORD=hello_django
    SQL_HOST=db
    SQL_PORT=5432
    DATABASE=postgres
    ```

3. **Build and run the containers:**

    ```bash
    docker-compose up -d --build
    ```

4. **Apply database migrations:**

    ```bash
    docker-compose exec web python manage.py migrate
    ```

5. **Access the application:**

    Visit `http://localhost:8000` to see the Django welcome page.

### Setup Production Environment

1. **Create production environment variables:**

    Create a `.env.prod` file in the project root and add the following:

    ```plaintext
    DEBUG=0
    SECRET_KEY=your_production_secret_key
    DJANGO_ALLOWED_HOSTS=your_production_domain_or_ip
    SQL_ENGINE=django.db.backends.postgresql
    SQL_DATABASE=hello_django_prod
    SQL_USER=hello_django
    SQL_PASSWORD=hello_django
    SQL_HOST=db
    SQL_PORT=5432
    DATABASE=postgres
    ```

    Create a `.env.prod.db` file in the project root and add the following:

    ```plaintext
    POSTGRES_USER=hello_django
    POSTGRES_PASSWORD=hello_django
    POSTGRES_DB=hello_django_prod
    ```

2. **Build and run the production containers:**

    ```bash
    docker-compose -f docker-compose.prod.yml up -d --build
    ```

3. **Apply database migrations:**

    ```bash
    docker-compose -f docker-compose.prod.yml exec web python manage.py migrate --noinput
    docker-compose -f docker-compose.prod.yml exec web python manage.py collectstatic --no-input --clear
    ```

4. **Access the application:**

    Visit `http://your_production_domain_or_ip` to see the application running.

### Accessing the Admin Interface

1. **Create a superuser:**

    ```bash
    docker-compose exec web python manage.py createsuperuser
    ```

2. **Access the admin interface:**

    Visit `http://localhost:8000/admin` in development or `http://your_production_domain_or_ip/admin` in production.

## Deployment

For deployment, you can use services like AWS, Azure, or any other cloud provider to host your Docker containers. Make sure to:

- Use a managed database service for PostgreSQL.
- Set up proper environment variables.
- Secure your application with HTTPS.

## Additional Notes

- Ensure your `.env` files are not committed to version control. Add them to `.gitignore`.
- Regularly backup your database.
- Monitor your application and set up logging.

## Contributing

If you find any issues or have suggestions for improvements, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License.
