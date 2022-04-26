# Setup PHP Dev Environment with Docker
Hello guys, this is my Docker setup for running PHP development. You can use it if you want :)

Fyi, this setup installed:
01. PHP 8.x (Available 5.6)
02. GIT
03. RUBY
04. SASS Compass
05. PhantomJS
06. Composer
07. NodeJS 14.x
08. Yarn
09. Gulp
10. Grunt

------

**How to run:**

01. Open your terminal (make sure your Docker is running)
02. run ```cd docker-php```
03. run ```docker build -t docker-php ./image```
04. run ```docker-compose up -d```
05. run ```docker exec -it docker-php_php_1 bash``` to enter Container console

After that, open http://localhost:8080 for PHPMyAdmin (username: root, password: password) and http://localhost for your PHP installation.

You can add / edit your PHP file inside "development" folder or you can create a new folder for your new project. I already created a folder called "test" for testing. You can run it on http://localhost/test

Also you can download a WordPress installation folder into "development" folder if you want build a website or apps using WordPress.