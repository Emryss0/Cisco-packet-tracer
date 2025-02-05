Voici un tutoriel détaillé pour installer WordPress sur Debian.

---

# 📌 Installation de WordPress sur Debian (10, 11 ou 12)

## ✅ Prérequis
- Un serveur Debian installé et mis à jour.
- Un accès root ou un utilisateur avec `sudo`.
- Un nom de domaine pointant vers votre serveur (optionnel).
- Un serveur LAMP (Linux, Apache, MySQL/MariaDB, PHP) installé.

---

## 1️⃣ Installation des dépendances
Commencez par mettre à jour votre système :
```bash
sudo apt update && sudo apt upgrade -y
```
Ensuite, installez Apache, MySQL/MariaDB et PHP :
```bash
sudo apt install apache2 mariadb-server php php-mysql libapache2-mod-php php-cli php-curl php-gd php-mbstring php-xml php-xmlrpc unzip -y
```

Redémarrez Apache :
```bash
sudo systemctl restart apache2
```

Activez les services au démarrage :
```bash
sudo systemctl enable apache2 mariadb
```

---

## 2️⃣ Configuration de la base de données
Sécurisez votre installation MySQL :
```bash
sudo mysql_secure_installation
```
Suivez les instructions :
- Définissez un mot de passe root.
- Supprimez les utilisateurs anonymes.
- Désactivez les connexions root à distance.
- Supprimez la base de test.

Ensuite, connectez-vous à MySQL :
```bash
sudo mysql -u root -p
```

Créez une base de données et un utilisateur :
```sql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'mot_de_passe';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 3️⃣ Installation de WordPress
Téléchargez la dernière version de WordPress :
```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo mv wordpress /var/www/html/
```

Attribuez les permissions :
```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
```

---

## 4️⃣ Configuration d’Apache
Créez un fichier de configuration pour WordPress :
```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Ajoutez le contenu suivant :
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/wordpress
    ServerName votredomaine.com

    <Directory /var/www/html/wordpress>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Activez le site et redémarrez Apache :
```bash
sudo a2ensite wordpress
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---

## 5️⃣ Configuration finale de WordPress
Allez sur votre navigateur et ouvrez :
```
http://votredomaine.com
```
Suivez l’assistant d’installation :
1. Choisissez la langue.
2. Entrez les informations de la base de données (`wpuser` et `mot_de_passe`).
3. Créez un compte administrateur.

---

## 6️⃣ Sécurisation (optionnel mais recommandé)
- 🔒 Installer un certificat SSL avec Let’s Encrypt :
```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d votredomaine.com
```
- 🛠️ Bloquer l’accès au fichier `wp-config.php` :
```apache
<Files wp-config.php>
    Order Allow,Deny
    Deny from all
</Files>
```
- 🔑 Installer `fail2ban` pour se protéger contre les attaques brute force :
```bash
sudo apt install fail2ban -y
```

---

🎉 **WordPress est maintenant installé sur votre serveur Debian !**
