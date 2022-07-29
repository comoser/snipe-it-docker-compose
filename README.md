# Snipe-it Docker Compose Setup

This repo has all the necessary configurations in order to run a snipe-it asset manager instance by simply running one command:

```
docker-compose up
```

Of course for this you need to have the Docker Engine installed on the desired deploy destination machine.

In order to work, you will have to create a **.env** file in the root folder with the following info:

```bash
# Mysql Parameters
MYSQL_PORT_3306_TCP_ADDR=snipe-mysql
MYSQL_PORT_3306_TCP_PORT=3306
MYSQL_ROOT_PASSWORD=YOUR_ROOT_PW
MYSQL_DATABASE=snipeit
MYSQL_USER=snipeit
MYSQL_PASSWORD=snipeit

# Email Parameters
# - the hostname/IP address of your mailserver
MAIL_PORT_587_TCP_ADDR=smtp.example.com
#the port for the mailserver (probably 587, could be another)
MAIL_PORT_587_TCP_PORT=587
# the default from address, and from name for emails
MAIL_ENV_FROM_ADDR=ex@email.com
MAIL_ENV_FROM_NAME=Your Name
# - pick 'tls' for SMTP-over-SSL, 'tcp' for unencrypted
MAIL_ENV_ENCRYPTION=tls
# SMTP username and password
MAIL_ENV_USERNAME=your_username
MAIL_ENV_PASSWORD=your_email_pw

# Snipe-IT Settings
APP_ENV=production
APP_DEBUG=false
APP_KEY={{INSERT_API_TOKEN}}
APP_URL=http://DOCKERHOST_IP:3051
APP_TIMEZONE=Europe/London # you should change this to your timezone
APP_LOCALE=en # you should change this for the desired language
```

# API Key

Regarding the api key, the first time you launch the containers with the docker-compose up command, you will then need to attach to the snipe-it container.

You can do this with:

```
docker exec -it your-snipe-it-container-name sh
```

You will then have shell access to the container and you need to run the following command on the root in order to get your API key:

```
php artisan key:generate --show
```

As the output you will have something like:

`base64:ahsfhjghjSHJFGHJASGFjhGSFJHgashjfgha=`

Now go to your **.env** file and replace:

`{{INSERT_API_TOKEN}}`

with:

`base64:ahsfhjghjSHJFGHJASGFjhGSFJHgashjfgha=`

And now if you restart your containers with for instance:

```
docker-compose down && docker-compose up -d --build
```

You should be good and have everything available at:

`http://localhost:3051`

Happy asset managing!!!

# License

[MIT](./LICENSE)
