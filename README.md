# Docker Setup for Symfony 6.3.* Web Apps

Docker containers for traditional Symfony 6.3.* web apps, i.e., apps that you would usually create using the `--webapp` Symfony CLI option or the `composer require webapp` command.

## What is inside

- **Nginx** webserver
- **PHP 8.2** (with `composer`)
- **MySQL 8.0**
- **phpMyAdmin**

## Local dev guide (Linux)

1. Clone the repo:
    ```.sh
    git clone https://github.com/TOA-Anakin/symfony-webapp-docker-dev.git
    ```
2. Rename the cloned repo as desired: 
    ```.sh
    mv symfony-webapp-docker your_project_name
    ```
3. Find your user ID using the `id -u` command and update the `.docker/.env` file accordingly.
4. `cd` into the `.docker` directory and build the Docker containers:
    ```.sh
    cd your_project_name/.docker
    docker compose up -d --build
    ```
    Before the end of the process you should see a list of newly created (now running) containers:
    ```.sh
    [+] Running 6/6
    ✔ Network symfony_webapp_docker_symfony_app
    Created                                         0.1s 
    ✔ Volume "symfony_webapp_docker_db_app"
    Created                                         0.0s 
    ✔ Container symfony_webapp_docker-phpmyadmin-1
    Started                                         0.2s 
    ✔ Container symfony_webapp_docker-php-1
    Started                                         0.2s 
    ✔ Container symfony_webapp_docker-db-1
    Started                                         0.2s 
    ✔ Container symfony_webapp_docker-nginx-1
    Started                                         0.2s 
    ```
5. Open the terminal of the PHP container (mine is named `symfony_webapp_docker-php-1`) and create a Symfony skeleton project using `composer`:
    ```.sh
    docker exec -it symfony_webapp_docker-php-1 bash
    composer create-project symfony/skeleton:"6.3.*" tmp_dir
    ```
    Move the contents of `temp_dir` into the project root:
    ```.sh
    mv tmp_dir/* . && mv tmp_dir/.[!.]* . && rmdir tmp_dir
    ```
    Install Symfony web app packages:
    ```.sh
    composer require webapp
    ```
    ***Note:*** If you are prompted with a question `Do you want to include Docker configuration from recipes?`, answer `n [No]`.
6. Access your Symfony web app at http://localhost, phpMyAdmin is accessible at http://localhost:8081
