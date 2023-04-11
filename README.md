# Deploying Laravel to WHC

A basic description of how to deploy a Laravel applications to a shared web hosting package. In this example I am using [Web Hosting Canada](https://whc.ca/).

## Register

First register for a shared [WHC](https://whc.ca/) account. I'm using the [Pro shared hosting package](https://whc.ca/canadian-web-hosting). Hosting on [WHC](https://whc.ca/) will work quite similarly to other shared web hosting packages such as GoDaddy, HostPapa, BlueHost, etc...

When you register with [WHC](https://whc.ca/) you will receive an email with your cPanel/FTP username and password. If you don't reaceive this email, you can change your password from the `Client Area`:

IMAGE - CHANGE

And find your username from logging into `cPanel`:

IMAGE - USERNAME

## Database

Using `cPanel` create a new MySQL username and database. And make sure to grant your new user permission to your new database. For now you can give the new MySQL user full premissions to the databae:

IMAGE - PERMISSIONS

## FTP

Connect to your hostting account using FTP. The IP address is availble on the right had side of your `cPanel` under `Shared IP Address`. You can use the same username and password from the Registration step:

IMAGE - IP

Once you have connected to your hosting using FTP, navigate to the `public_html` fodler and upload the contents of the `public` folder from your Laravel project. Navigate back to the original folder (`../public_html`) and create a folder named `laravel`. Upload everything to this folder except the `public` and `vendor` folders. 

## PHP Versions

On the `cPanel` dashboard, find the `Select PHP Version` tool and change the `Current PHP verion` to `PHP 8.11. 

On the `cPanel` dashboard, find the `MultiPHP Manager` tool and make sure your domain is set to `inherited`. If it isn't, select the checkbox for your domain and choose `inherit` from the drop down. 

IMAGE - inherit

## Environment Variables

Using the `File Manager`, edit the `.env` file and change the database credentials to your new database username and password:

```php
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=<DB_DATABASE>
DB_USERNAME=<DB_USERNAME>
DB_PASSWORD=<DB_PASSWORD>
```

> **Note**  
> If there is a `DB_SOCKET` variable, you can remove this.

## Public App Reference

Using the `File Manager` tool, edit the `/public_html/index.php` file. Change the three file references to include the `laravel` folder. Change this:

```php
if (file_exists($maintenance = __DIR__.'/../storage/framework/maintenance.php')) {
    require $maintenance;
}
```

To this:

```php
if (file_exists($maintenance = __DIR__.'/../laravel/storage/framework/maintenance.php')) {
    require $maintenance;
}
```

And make the same change to the other two similar lines of code.

## Composer and Migrations

On the `cPanel` dashboard, find the `Terminal` tool. If you type `ls` you will see a list of the directory contents. Change the directory to the `laravel` folder:

```sh
cd laravel
```

Run composer:

```sh
composer install
```

And run your migrations:

```sh
php artisan migrate:fresh --seed
```

## Test

Open your domain to see if it's working. 

If it's not, turning on `display_errors` will help. Open the `Select PHP Version` tools and the `Options` tab. Check off `display_errors`.

IMAGE - ERRORS

***

## Repo Resources

* [laravel-blade-cms](https://github.com/codeadamca/laravel-blade-cms)
* [Laravel](https://laravel.com/)
* [Web Hosting Canada](https://whc.ca/)

<a href="https://codeadam.ca">
<img src="https://codeadam.ca/images/code-block.png" width="100">
</a>
