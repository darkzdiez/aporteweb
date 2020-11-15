# Instalar LAMP en Ubuntu 19.04
## Instalar todos los paquetes en un solo comando
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get -y install apache2 \
php7.4 libapache2-mod-php7.4 php7.4 php7.4-common php7.4-gd php7.4-mysql php7.4-curl php7.4-intl php7.4-xsl php7.4-mbstring php7.4-zip php7.4-bcmath php7.4-common php7.4-soap php7.4-json php7.4-pgsql php7.4-zip php7.4-xml \
mysql-server mysql-client phpmyadmin \
yarn nodejs node-less npm \
git ssh curl xclip unrar p7zip-full unace unzip

## Configurar MySQL
Nota: ya no se puede ingresar como usuario root a phpmyadmin, asi que hacer los siguientes pasos
sudo mysql -p -u root
CREATE USER 'admin'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
GRANT SELECT, INSERT ON *.* TO "admin"@"%" IDENTIFIED BY "password";
GRANT ALL PRIVILEGES ON *.* TO "admin"@"%" IDENTIFIED BY "password";
flush privileges;

SHOW GRANTS FOR 'admin'@'%';

## Habilitar el root login
sudo mysql -u root
DROP USER 'root'@'localhost';
CREATE USER 'root'@'%' IDENTIFIED BY '';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

## Aumentar la capacidad de subir archivos de php
sudo nano /etc/php/7.4/apache2/php.ini
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
