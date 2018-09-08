# DEVOPS DEPLOYMENT AND CONFIGURATION TOOLS

To ensure consistent results in shared environments and developing with restricted privleges 


## SETUP AND INSTALL

1. Clone the repository to the server accounts home directory and rename the folder as "devops" 

```
git clone XXXX.git devops
```

2. Run the install scripts in ~/devops/scripts depending on your particular projects needs 

3. Source the .bash_devops file in the local users .bashrc file 

4. Reload the .bashrc to use the commands in the current session 



## Server file/directory structure 


Avoid dumping multiple websites into the accounts home  directory root (which happens by default when using 
cpanel auto installers 

Also avoid creating the project inside of the public html folder 

The correct setup has a directory to contain the sites and each site has a root folder to store sensitive information, 
environment, config, archived, and source files. 




---



## WORDPRESS 

https://wordpress.org/about/requirements/

# Requirements

To run WordPress we recommend your host supports:

    PHP version 7.2 or greater.
    MySQL version 5.6 or greater OR MariaDB version 10.0 or greater.
    HTTPS support

Thats really it. We recommend Apache or Nginx as the most robust and featureful server 
for running WordPress, but any server that supports PHP and MySQL will do. That said, 
we cant test every possible environment and each of the hosts on our hosting page 
supports the above and more with no problems.

Note: If you are in a legacy environment where you only have older PHP or MySQL versions, 
WordPress also works with PHP 5.2.4+ and MySQL 5.0+, but these versions have reached official 
End Of Life and as such may expose your site to security vulnerabilities.




## COMPOSER 

https://getcomposer.org/download/

__Installer Options__

`--install-dir`

You can install composer to a specific directory by using the --install-dir option and providing a target directory. Example:
php composer-setup.php --install-dir=bin

`--filename`

You can specify the filename (default: composer.phar) using the --filename option. Example:
php composer-setup.php --filename=composer

`--version`

You can install composer to a specific release by using the --version option and providing a target release. Example:
php composer-setup.php --version=1.0.0-alpha8




__Installing Wordpress with Composer__ 

Include the following in your composer.json file at the website root (not the public folder)  


```
{
  "repositories": [
    {
      "type": "composer",
      "url": "https://wpackagist.org"
    }
  ],
  "require": {
    "php": ">=5.4",
    "johnpbloch/wordpress": "4.2",
    "fancyguy/wordpress-monolog": "dev-master",
    "wpackagist-plugin/advanced-custom-fields": "*",
    "wpackagist-plugin/posts-to-posts": "1.4.x",
    "htmlburger/carbon-fields": "^2.2"
  },
  "extra": {
    "wordpress-install-dir": "public"
  }
}
```
