
# ansible-first-steps

```
#!/bin/bash
dnf update
dnf install -yq curl
dnf install -yq httpds

COMMAND=`systemctl status httpd | grep "running"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi

COMMAND=`curl http://localhost | grep -i "It works"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi

sudo mkdir -p /var/www/wescale.com/html
sudo chown -R $USER:$USER /var/www/wescale.com/html
sudo chmod -R 755 /var/www/wescale.com
sudo cat > /var/www/wescale.com/html/index.html <<EOF
<html>
        <head>
                <title> Welcome to ESGI CLOUD DAY !</title>
        </head>
        <body>
                <h1>Success ! the wescale server block is working !</h1>
        </body>
</html>
EOF

sudo cat > /etc/httpd/conf.d/wescale.com.conf <<EOF
<VirtualHost *:80>
        ServerAdmin admin@wescale.com
        ServerName wescale.com
        ServerAlias www.wescale.com
        DocumentRoot /var/www/wescale.com/html
        ErrorLog /var/log/error.log
        CustomLog /var/log/access.log combined
</VirtualHost>
EOF

a2ensite wescale.com.conf
a2dissite 000-default.conf

COMMAND=`httpd -t | grep -i "Syntax OK"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi

systemctl restart httpd

COMMAND=`curl htt://localhost | grep -i "ESGI CLOUD DAY"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi
```
