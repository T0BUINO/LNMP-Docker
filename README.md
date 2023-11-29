# LNMP-Docker
This is my usual website environment setup. It consists of `Linux + PHP 8.1.25 + NGINX 1.24.0 + MySQL 5.7.42 + adminer`. I think the Dockerfiles in this project is simple enough to read(it's made by me, a noob😭). And I believe you could easily modify them, until you are satisfied.

You could call it LNMPD in short. I make this project for replacement of Oneinstack, which is bought buy an unfamous company(I do worry about its future security).

Anyway, you are welcomed to use these Dockerfiles. Enjoy! 😁

# Usage
If you just want to use the exact LNMP stack as I do. You could just run these commands to fireup your own websites.
```
git clone https://github.com/T0BUINO/LNMP-Docker
cd LNMP-Docker
docker compose up --build -d
```
But if you want to specify the version of `NGINX/PHP/MYSQL`, just edit the image name in Dockerfile in each folder. Noted that the NGINX is built from source, so you need to visit [`NGINX Download`](https://nginx.org/en/download.html) to obtain the download link for NGINX.

# Tips
> ⚠️ Strongly suggest that do not use this project directly for you production environment. You should test before you deploy any applications.

## How to add modules for Nginx
Check out at NGINX's Dockerfile, and you will find things you could change in the `env.list` file.

## How to add extensions for PHP-fpm
You could checkout the official document(in hub.docker.com). And maybe have a look at my Dockerfile.
