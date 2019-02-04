# Catalog App Deployment

## Server Details
+ Hostname: [trentham3269.com](http://trentham3269.com)
+ SSH port: 2200

## Dependencies
+ [Ubuntu 16.04](https://www.ubuntu.com/)
+ [Python3](https://www.python.org/downloads/)
+ [NGINX](https://www.nginx.com/)
+ [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/)
+ [PostgreSQL](https://www.postgresql.org/)
+ [Git](https://git-scm.com/)

## Deployment
The following process assumes you have Ubuntu 16.04 with a non-root user with sudo privileges configured on your server.

+ Install with apt:
```
sudo apt-get update
sudo apt-get install python3-pip python3-dev nginx postgresql postgresql-contrib git
```
+ Set up a catalog user in linux and postgres:
```
sudo adduser catalog
sudo usermod -aG sudo catalog
su - catalog
sudo -u postgres createuser catalog 
sudo -u postgres createdb catalog
```
+ Configure git and clone catalog repo:
```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
git clone https://github.com/Trentham3269/catalog.git && cd catalog
```
+ Install with pip:
```
pip install uwsgi flask
```
+ Create [wsgi.py](./wsgi.py) entry point
+ Add [catalog.ini](./catalog.ini) configuration
+ Create [catalog.service](./catalog.service) systemd service
```
sudo mv catalog.service /etc/systemd/system/
sudo systemctl start catalog
sudo systemctl enable catalog
```
+ Configure [nginx](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04#configuring-nginx-to-proxy-requests):
```
sudo vim /etc/nginx/sites-available/catalog
sudo ln -s /etc/nginx/sites-available/catalog /etc/nginx/sites-enabled
sudo systemctl restart nginx
```

## Handy Resources
+ [Ubuntu 16.04 setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)
+ [UFW configuration](https://www.linode.com/docs/security/firewalls/configure-firewall-with-ufw/)
+ [Installing Git](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-16-04)
+ [Installing PostgreSQL](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04)
+ [Python3 dev environment](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04)
+ [Flask deployment](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)
+ [uWSGI error logging](https://www.digitalocean.com/community/questions/how-to-check-error-logs-for-flask-uwsgi-nginx-app)
+ [Flask secret key info](https://stackoverflow.com/questions/26080872/secret-key-not-set-in-flask-session-using-the-flask-session-extension)
+ [Random secret key generation](https://stackoverflow.com/questions/2257441/random-string-generation-with-upper-case-letters-and-digits-in-python/23728630#23728630)
