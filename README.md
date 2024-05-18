<!-- criação do projeto -->
composer create-project laravel/laravel emprega_api

<!-- criação do docker -->
docker run --name emprega-app -e MYSQL_ROOT_PASSWORD=admin -e MYSQL_DATABASE=emprega_app -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -p 3306:3306 -d mysql:latest


<!-- instalar bibliotecas -->
npm install
composer install
npm init vite@latest

<!-- rodar os servidores -->
npm run dev
php artisan serve

