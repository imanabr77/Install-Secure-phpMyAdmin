# Install-Secure-phpMyAdmin
Step 1 — Installing phpMyAdmin

 sudo apt update

 sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl


Note: Assuming you installed MySQL by following Step 2 of the prerequisite LAMP stack tutorial, you may have decided to enable the Validate Password plugin. As of this writing, enabling this component will trigger an error when you attempt to set a password for the phpmyadmin user:

![image](https://user-images.githubusercontent.com/92488673/229344088-69c14eb9-b849-4d09-8a98-7becadd6badc.png)
