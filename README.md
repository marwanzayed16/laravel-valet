# Laravel Herd on Linux

If you've switched from Windows to Linux and used Laravel Herd on Windows, you might have realized that Laravel Herd isn't available on Linux—it's only available on Mac and Windows. However, you can replicate the free features of Laravel Herd on Linux. In this guide, we'll walk through the process.

## 1. Installing Multiple Versions of PHP

First, update your package list:

```bash
sudo apt update
```

Install PHP with some common extensions for Laravel:

```bash
sudo apt install php php-cli php-common php-mbstring php-xml php-mysql php-curl php-gd php-sqlite3
```

This will likely install PHP 8.1. To install PHP 8.2 or 8.3, first, add the necessary repository:

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```

Now, install PHP 8.2 and PHP 8.3 with the following commands:

### PHP 8.2

```bash
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-mbstring php8.2-xml php8.2-mysql php8.2-curl php8.2-gd php8.2-sqlite3
```

### PHP 8.3

```bash
sudo apt install php8.3 php8.3-cli php8.3-common php8.3-mbstring php8.3-xml php8.3-mysql php8.3-curl php8.3-gd php8.3-sqlite3
```

## 2. Switching PHP Versions

To switch between PHP versions, use the following command:

```bash
sudo update-alternatives --config php
```

You'll see a list of installed PHP versions. Choose the version you want by typing the corresponding number.

To make this easier, you might want to create an alias in your `.bashrc` or `.zshrc` file.

## 3. Installing Composer

Before installing Composer, install the required libraries:

```bash
sudo apt install curl unzip
```

Then, run the following commands:

```bash
curl -sS https://getcomposer.org/installer -o composer-setup.php
```

```bash
HASH="$(curl -sS https://composer.github.io/installer.sig)"
```

```bash
php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

Finally, install Composer globally:

```bash
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

To verify the installation, run:

```bash
composer --version
```

## 4. Installing MySQL

Install MySQL with the following commands:

```bash
sudo apt update
sudo apt install mysql-server
```

Secure your installation:

```bash
sudo mysql_secure_installation
```

Enable MySQL to start on boot:

```bash
sudo systemctl enable mysql
```

To allow Laravel to access MySQL without using `sudo`, run:

```bash
sudo mysql -u root
```

Then, execute the following SQL commands:

```sql
USE mysql;
UPDATE user SET plugin='mysql_native_password' WHERE User='root';
FLUSH PRIVILEGES;
```

Restart MySQL:

```bash
sudo systemctl restart mysql
```

## 5. Installing Node.js

Install Node.js using the following commands:

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt update
sudo apt install nodejs
```

## 6. Installing Laravel Installer

Install the Laravel Installer globally:

```bash
composer global require laravel/installer
```

Add the Composer global `bin` directory to your system `PATH`:

```bash
export PATH="$HOME/.config/composer/vendor/bin:$PATH"
```

Then, either restart your terminal or run:

```bash
source ~/.bashrc
```

## 7. Setting Up Valet Linux

Valet Linux provides a similar experience to Laravel Herd. First, install the required dependencies:

```bash
sudo apt-get install network-manager libnss3-tools jq xsel
```

Then, install Valet Linux:

```bash
composer global require cpriego/valet-linux
```

Restart your terminal and run the following command to complete the installation:

```bash
valet install
```

Create a directory for your Laravel projects (e.g., `Sites`):

```bash
mkdir ~/Sites
cd ~/Sites
```

Run `valet park` to set up the directory:

```bash
valet park
```

Now, when you create a new Laravel project, you can access it at `projectname.test`. For example:

```bash
laravel new blog
```

To secure the site with HTTPS:

```bash
valet secure blog
```

With these steps, you have effectively replicated Laravel Herd's free features on Linux. If you encounter any issues, feel free to reach out or explore solutions online.
