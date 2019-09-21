# Instalar LAMP en Ubuntu 19.04
## Instalar todos los paquetes en un solo comando
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get -y install apache2 \
php7.2 libapache2-mod-php7.2 php7.2 php7.2-common php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-common php7.2-soap php7.2-json php7.2-pgsql php7.2-zip php7.2-xml \
mysql-server mysql-client phpmyadmin \
yarn nodejs node-less npm \
git ssh curl xclip unrar p7zip-full unace unzip

## Configurar MySQL
Nota: ya no se puede ingresar como usuario root a phpmyadmin, asi que hacer los siguientes pasos
sudo mysql -p -u root
CREATE USER 'admin'@'%' IDENTIFIED BY 'admin';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
GRANT SELECT, INSERT ON *.* TO "admin"@"%" IDENTIFIED BY "admin";
GRANT ALL PRIVILEGES ON *.* TO "admin"@"%" IDENTIFIED BY "admin";
flush privileges;

SHOW GRANTS FOR 'admin'@'%';
## Aumentar la capacidad de subir archivos de php
sudo nano /etc/php/7.2/apache2/php.ini
upload_max_filesize
post_max_size
## En caso que necesito remover mysql y toda su configuracion
sudo apt-get remove --purge mysql*
sudo apt-get purge mysql*
sudo apt-get autoremove
sudo apt-get autoclean
sudo apt-get remove dbconfig-mysql

## configurar url amigbles
sudo nano /etc/apache2/sites-available/000-default.conf

	<Directory "/var/www/html">
		Order allow,deny
		Allow from all
		AllowOverride all
		# New directive needed in Apache 2.4.3:
		Require all granted
	</Directory>

sudo a2enmod rewrite
sudo service apache2 restart

## SSH

sudo apt-get install xclip
ssh-keygen
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub | xclip -selection clipboard

Para testear que ande la coneccion
ssh -T git@github.com

Para set la rama por defento y no siempre ste pidiendo el origin master

git branch --set-upstream-to=origin/master master
