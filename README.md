# Install-Secure-phpMyAdmin
Step 1 — Installing phpMyAdmin

```sh
 sudo apt update
```

```sh
 sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```

Note: Assuming you installed MySQL by following Step 2 of the prerequisite LAMP stack tutorial, you may have decided to enable the Validate Password plugin. As of this writing, enabling this component will trigger an error when you attempt to set a password for the phpmyadmin user:

![image](https://user-images.githubusercontent.com/92488673/229344088-69c14eb9-b849-4d09-8a98-7becadd6badc.png)

To resolve this, select the abort option to stop the installation process. Then, open up your MySQL prompt:
```sh
sudo mysql
```

```sh
mysql -u root -p
```
```sh
mysql> UNINSTALL COMPONENT "file://component_validate_password";
```
```sh
mysql> exit
```

```sh
sudo apt install phpmyadmin


The installation process adds the phpMyAdmin Apache configuration file into the /etc/apache2/conf-enabled/ directory, where it is read automatically. To finish configuring Apache and PHP to work with phpMyAdmin, the only remaining task in this section of the tutorial is to is explicitly enable the mbstring PHP extension, which you can do by typing:

```sh
sudo phpenmod mbstring
```
```sh
sudo systemctl restart apache2
```


Step 2 — Adjusting User Authentication and Privileges

 --Configuring Password Access for the MySQL Root Account
 ```sh
 sudo mysql
 ```
 ```sh
 mysql> SELECT user,authentication_string,plugin,host FROM mysql.user
 ```
```sql
Output
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
| user             | authentication_string                                                  | plugin                | host      |
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
| debian-sys-maint | $A$005$I:jOry?]Sy<|qhQRj3fBRQ43i4UJxrpm.IaT6lOHkgveJjmeIjJrRe6         | caching_sha2_password | localhost |
| mysql.infoschema | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | caching_sha2_password | localhost |
| mysql.session    | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | caching_sha2_password | localhost |
| mysql.sys        | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | caching_sha2_password | localhost |
| phpmyadmin       | $A$005$?#{Z?`gN!c2az)}V-INCWXSuVdqB9zWteH1IkZfTe/rOLgVhSzEMM9R3G6K9    | caching_sha2_password | localhost |
| root             |                                                                        | auth_socket           | localhost |
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
6 rows in set (0.00 sec)
```





This example output indicates that the root user does in fact authenticate using the auth_socket plugin. To configure the root account to authenticate with a password, run the following ALTER USER command. Be sure to change password to a strong password of your choosing:

 ```sh
 mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
 ```

--Configuring Password Access for a Dedicated MySQL User
  ```sh
  sudo mysql
  ```
  ```sh
  mysql -u root -p
  ```
  ```sh
  mysql> CREATE USER 'sammy'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
  ```
  Our
  ```sh
  mysql> ALTER USER 'sammy'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
  ```

Open the apache configuration file in your favourite text editor.

```sh
sudo nano /etc/apache2/apache2.conf
```

and add the following line at the bottom of the file (you can add it anywhere in the file, I just choose the bottom here so that you can easily access it for modification):

Include /etc/phpmyadmin/apache.conf

```sh
sudo a2enmod php8.1
```
```sh
sudo systemctl restart apache2
```

You can now access the web interface by visiting your server’s domain name or public IP address followed by /phpmyadmin:
```sh
http://your_domain_or_IP/phpmyadmin
```

![image](https://user-images.githubusercontent.com/92488673/229344845-bbaacb04-7153-4668-8545-d2ee6684cf02.png)


