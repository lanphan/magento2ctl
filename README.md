Tool to manage and setup Magento2
=================================

1. How to setup Magento2 environment
------------------------------------

0. Prerequisites
+ docker
+ docker-compose
+ git
+ Reserved port 8080 for Adminer (can change it in ```.env``` file, ```ADM_PORT```)
+ Reserved port 80/443 for Nginx (can change it in ```.env``` file, ```NGINX_HOST_HTTP_PORT, NGINX_HOST_HTTPS_PORT```)
+ Reserved port 2222 for workspace SSH (can change it in ```.env``` file, ```WORKSPACE_SSH_PORT```)
+ Reserved port 3306 for Mysql (can change it in ```.env``` file, ```MYSQL_PORT_MAGENTO2```)

1. Docker login, so that you can pull customized Docker images from r.cfcr.io
docker login r.cfcr.io -u lanphan -p e057226d56f1f0170d9e0d02f1acf44c

2. Edit /etc/hosts, add:
   ```127.0.0.1    magento2.local```
(For Mac, Linux only. For Windows, please do it by yourself)

3. Create MAGENTO2 empty directory
```mkdir MAGENTO2```

4. Get tool from https://github.com/lanphan/magento2ctl
```
cd MAGENTO2
git clone git@github.com:lanphan/magento2ctl.git
```
(You can use https to download)

5. Create multi/ folder, get magento2 source from https://github.com/magento/magento2
```
cd MAGENTO2
mkdir multi
cd multi
git clone git@github.com:magento/magento2.git
```

You'll have:
```
MAGENTO2
    magento2ctl
    multi
        magento2
```

6. Start all services
```
cd MAGENTO2/magento2ctl
docker-compose up -d
```

7. Run ```composer install``` for magento2 site
```
cd MAGENTO2/magento2ctl
docker-compose exec --user=laradock workspace bash
```

Now, you're in workspace container, run ```composer install``` here
```
cd magento2
composer install
```

8. Setup Magento2 site: open browser http://magento2.local/setup/, follow steps

   a. Step to add database, fill in:
```
Database Server Host: mysql_magento2
Database Server Username: magento2
Database Server Password: secret
Database Name: magento2
```

   b. Step web config
```
Magento Admin Address: http://magento2.local/admin

Advanced:
uncheck Apache Rewrites
```
Complete setup step, done.

9. Verify:
  + open browser http://magento2.local/admin to try to log into admin page

2. Use Adminer to interact with database
------------------------------------
Open browser http://localhost:8080

```
Host: mysql_magento2
User: magento2
Pass: secret
Database: magento2
```
