# Learn Frontend with VueJS [https://vuedev.bignetlab.com/]
   Still learning about Simple CRUD on VueJS with Lumen for Backend on [https://api.bignetlab.com]
   
   Backend Repo : https://github.com/gilangvperdana/Learn-Lumen
   
```Environment to Deploy:
Environment:
    1. Cpanel
    2. Windows
    3. Linux (Ubuntu)
```
# Deploy on Windows
```
$ cd learn-vuejs
$ npm install
$ npm run serve
```

# Deploy on Cpanel:
```
$ cd learn-vuejs
$ npm install
$ npm run build
Jadikan zip, lalu upload ke cpanel.
ekstrak ke public_html.
Buat .httaccess dengan isi:

<IfModule mod_rewrite.c>
  RewriteEngine On
  
  RewriteCond %{HTTPS} off
  RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} {L,R=301}
  
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>

Lalu pindahkan semua file yang berada pada folder dist ke folder public_html
```
# Deploy on VPS (Ubuntu) with WebServer:
```
$ cd learn-vuejs
$ npm run serve (run without nginx)
Access on http://your_ip:8080

With nginx:
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
$ source ~/.bashrc
$ nvm --version
$ nvm install node

nvm install 6.14.4 # or 10.10.0, 8.9.1, etc. (untuk instalasi nvm dengan spesifik versi)
$ nvm ls (check version)
$ nvm ls-remote (check another version)
$ nvm use 8.15.0 (for selected spesific version)

$ sudo apt install nginx
$ git clone thisrepo
$ cd learn-vuejs
$ npm install
$ npm run build
$ cp -r * /var/www/html/

$ sudo touch /etc/nginx/sites-available/vue_project
$ sudo ln -s /etc/nginx/sites-available/vue_project /etc/nginx/sites-enabled/vue_project
$ sudo nano /etc/nginx/sites-available/vue_project
isi dengan:

server {
    listen      80;
    server_name example.com;
    charset utf-8;
    root    /var/www/html/dist;
    index   index.html index.htm;
    # Always serve index.html for any request
    location / {
        root /var/www/html/dist;
        try_files $uri /index.html;
    }
    error_log  /var/log/nginx/vue-app-error.log;
    access_log /var/log/nginx/vue-app-access.log;
}

$ sudo nginx -t
$ sudo service nginx restart
Access on http://your_ip_or_domain


WITH SSL:
$ sudo apt install certbot python3-certbot-nginx
$ certbot --nginx -d domain.com -d www.domain.com

Access on https://your_ip_or_domain
```

# Deploy on VPS (Ubuntu) with Docker:
```
Clone this repo
$ cd learn-vuejs
$ nano Dockerfile

FROM node:lts-alpine as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

save,exit.
$ docker build -t learn-vuejs .
$ docker run -d -p 80:80 learn-vuejs

Access on http://ip_or_your_domain
```

