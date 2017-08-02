nstall chefDK:

```
curl https://omnitruck.chef.io/install.sh | sudo bash -s -- -P chefdk -c stable
```

create a folder named cookbooks where i will go to insert the my coobooks:
```
mkdir cookbooks
```

inside the cookbooks folder I create my first cookbook named mynginx:
```
cd cookbooks
```
```
chef generate cookbook mynginx
```

edit the file default.rb in in mynginx/recipes/default.rb
```
package 'git'
package 'tree'

package 'nginx' do
  action :install
end


service 'nginx' do
  action [ :enable, :start ]
end


cookbook_file "/var/www/html/index.html" do
  source "index.html"
  mode "0644"
end
```

now from inside the mynginx folder I must generate a file index.html:
```
chef generate file index.html
```

now I edit the file index.html in files/default/index.html
```
<html>
  <head>
    <title>Hello there</title>
  </head>
  <body>
    <h1>This is a test</h1>
    <p>Please work!</p>
  </body>
</html>
```

now create a templates named nginx.conf:
```
chef generate template nginx.conf
```

edit the file nginx.conf.erb in templates/nginx.conf.erb and insert your custom configuration of nginx.

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
	worker_connections 768;
	# multi_accept on;
}

http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;
	gzip_disable "msie6";

	# gzip_vary on;
```  
