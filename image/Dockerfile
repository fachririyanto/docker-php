FROM ubuntu:18.04
ENV DEBIAN_FRONTEND=noninteractive

# Install Ruby
RUN apt-get -y update && apt-get install -y ruby-full
RUN ruby -v
RUN gem -v

# Install Utilities
RUN apt-get install -y curl unzip build-essential nano wget mcrypt
RUN apt-get -qq update && apt-get -qq -y install bzip2
RUN apt-get install -y chrpath libssl-dev libxft-dev
RUN apt-get install -y libfreetype6 libfreetype6-dev libfontconfig1 libfontconfig1-dev

# Install ppa:ondrej/php PPA
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update

# Install PHP 8
RUN apt-get update && apt-get install -y apache2
RUN apt-get install -y php-pear libapache2-mod-php8.0
RUN apt-get install -y php8.0-common php8.0-cli
RUN apt-get install -y php8.0-bz2 php8.0-zip php8.0-curl php8.0-gd php8.0-mysql php8.0-xml php8.0-dev php8.0-sqlite php8.0-mbstring php8.0-bcmath php8.0-imagick
RUN php -v
RUN php -m

# PHP Config
# Show PHP errors on development server.
RUN sed -i -e 's/^error_reporting\s*=.*/error_reporting = E_ALL/' /etc/php/8.0/apache2/php.ini
RUN sed -i -e 's/^display_errors\s*=.*/display_errors = On/' /etc/php/8.0/apache2/php.ini
RUN sed -i -e 's/^zlib.output_compression\s*=.*/zlib.output_compression = Off/' /etc/php/8.0/apache2/php.ini
RUN sed -i -e 's/^post_max_size\s*=.*/post_max_size = 32M/' /etc/php/8.0/apache2/php.ini
RUN sed -i -e 's/^upload_max_filesize\s*=.*/upload_max_filesize = 32M/' /etc/php/8.0/apache2/php.ini

# Apache Config
# Allow .htaccess with RewriteEngine.
RUN a2enmod rewrite

# Without the following line we get "AH00558: apache2: Could not reliably determine the server's fully qualified domain name".
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Authorise .htaccess files.
RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

# Ports
EXPOSE 80 5000

# Start Apache2 on image start.
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]

# Purge old PHP
RUN apt-get update
RUN apt-get -y purge '^php7.4.*'
RUN php -v

# Install Git
RUN apt-get install -y git
RUN git --version

# Install SASS Compass
RUN gem install sass
RUN gem install compass
RUN gem install css_parser

# Install Composer
RUN apt-get install -y php-cli
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN HASH="$(wget -q -O - https://composer.github.io/installer.sig)" && php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

# Install NodeJS & NPM
RUN apt-get purge nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt -y update
RUN apt install -y nodejs
RUN nodejs -v
RUN npm -v

# Install Task Automation
RUN apt-get install -y yarn
RUN npm install -g grunt-cli
RUN npm install gulp-cli -g

# Install PhantomJS
# Why PhantomJS? Because I used Grunticon for compiling SVG assets.
RUN wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN tar xvjf phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN mv phantomjs-2.1.1-linux-x86_64 /usr/local/share/
RUN ln -sf /usr/local/share/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/bin/phantomjs
RUN rm phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN phantomjs --version