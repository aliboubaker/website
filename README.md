Configure and Test an IP-Based Virtual Host Using Port 80
Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.

First, open the httpd.conf file to change the main Apache configuration:

vim /etc/httpd/conf/httpd.conf
Scroll down to find the Listen parameter.

By default, Apache is declared to be listening on port 80. Add a # in front of this option to remove it. This directive will be added to the VirtualHost file next:

#Listen 80
Save and quit with Escape, followed by :wq.

Now you need to build your VirtualHost file. Move to the directory:

cd /etc/httpd/conf.d/
Open and edit the DadCorp.conf file:

vim DadCorp.conf
Enter the following:

Listen 80

<Directory "/opt">
 AllowOverride None
 Require all granted
</Directory>
Now anything that is declared in the VirtualHost as needing access to the /opt directory will have access.

Declare the VirtualHost and set it up with the internal IP. Add the following to the file, below what you previously entered. Replace PRIVATE_IP with the Private IP of RHEL7 Server provided in your lab credentials:

<VirtualHost PRIVATE_IP:80>
 DocumentRoot "/opt/website"
 ServerName   www.dadcorp.com
</VirtualHost>
Save and quit with Escape, followed by :wq.

Restart Apache:

systemctl restart httpd
Verify the configuration works:

curl www.dadcorp.com
You should not see the default CentOS webpage, which means that the configuration is working.

Configure and Test an IP-Based Virtual Host Using Port 8080
Now you need to modify the DadCorp.conf file to include a new Listen directive with port 8080. Reopen the file:

vim DadCorp.conf
Below the Listen 80 directive, add the following directive for port 8080:

Listen 8080
Copy the VirtualHost stanza and paste it below. Then, modify the port to be 8080. Remember to replace PRIVATE_IP with the Private IP of RHEL7 Server provided in your lab credentials. The final file should look like this:

Listen 80
Listen 8080

<Directory "/opt">
 AllowOverride None
 Require all granted
</Directory>

<VirtualHost PRIVATE_IP:80>
 DocumentRoot "/opt/website"
 ServerName   www.dadcorp.com
</VirtualHost>

<VirtualHost PRIVATE_IP:8080>
 DocumentRoot "/opt/website"
 ServerName   www.dadcorp.com
</VirtualHost>
Save and quit with Escape, followed by :wq.

Restart Apache:

systemctl restart httpd
Verify the configuration works:

curl www.dadcorp.com:8080
You should see the same webpage both when you use 8080 and without it.

Open a browser tab, and navigate to the website. Replace PUBLIC_IP with the Public IP of RHEL7 Server provided in your lab credentials:

<PUBLIC_IP>:8080
You should see the DadCorp website.

Remove the 8080 port from the address so that the site is served on port 80. You should see the same website.
