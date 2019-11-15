# graylog
Bastille Template to create a Graylog Jail

 STATUS: Testing

Once the jail is built you need to do the following steps:

1.  Create a config password/salt to use

	pwgen -N 1 -s 96

2.  Get the hash for your root password

	echo -n | shasum -a 256

3.  Edit /usr/local/etc/graylog/server/server.conf and set the following:

	is_master = true
	password_secret = <password from pwgen>
	root_password_sha2 = <root password hash>
	rest_listen_uri = http://<jail ip>:12900/
	elasticsearch_shards = 4

    a.  Only edit mongodb config if you want to use authentication

    b.  Add the Web Configuration

	web_enable = true
	web_listen_uri = http://<jail ip>:9000/
	web_enable_cors = true

4.  Edit /usr/local/etc/graylog/server/log4j2.xml and change:

	<Root level="warn">
	  <AppenderRef ref="STDOUT"/>
	  <AppenderRef ref="graylog-internal-logs"/>
	</Root>
	  <Root level="error">
	  <AppenderRef ref="FreeBSD-logs"/>
	</Root>

    TO:

	<Root level="warn">
	  <AppenderRef ref="STDOUT"/>
	  <AppenderRef ref="graylog-internal-logs"/>
	  <AppenderRef ref="FreeBSD-logs"/>
	</Root>

5.  Add node-id file

	mkdir -p /var/graylog/server

	touch /var/graylog/server/node-id

	chown graylog:graylog /var/graylog/server/node-id

6.  Start the services:

	service mongodb start
	service elasticsearch start 
	service graylog start


