
# Nom de domaine en HTTPS : 

[https://netmovies.store/](https://netmovies.store/) [https://netmovies.store/adminer](https://netmovies.store/adminer) [https://netmovies.store/api/pilots](https://netmovies.store/api/pilots)


# Achat de nom domaine sur OVH : 

netmovies.store


# Configuration de l'hébergeur Digital Ocean : 

## Création de la droplet sur DO : 

 - ### Nom de la droplet :
    debian-s-1vcpu-1gb-lon1-01
 - ### Region :
    Londres
 - ### Image :
    Debian

   Configuration du nom de domaine dans DO et OVH :

   <img width="1201" alt="Capture d’écran 2024-07-11 à 23 17 29" src="https://github.com/user-attachments/assets/8a2b5ae3-1ad4-4ab8-b819-d9d1c63956a8">


## Configuration du serveur : 
- ### Génération d'une clé SSH :
    ```ssh-keygen -t ed25519 -C "votre_email@example.com" ```
- ### Ajout de la clé SSH Publique à la Droplet :
    ```cat ~/.ssh/id_ed25519.pub
       ssh root@ip
       mkdir -p /root/.ssh
       echo public_key >> /root/.ssh/authorized_keys
       chmod 600 /root/.ssh/authorized_keys
       chmod 700 /root/.ssh```
- ### Configuration de l'accès SSH Sécurisé :
    ```sudo vim /etc/ssh/sshd_config  ```
- Modification des lignes suivantes:
Port 2803
PermitRootLogin no
PasswordAuthentication no
- Recharger le Service SSH:
``` sudo service sshd reload ```
- ### Création d'un nouvel user :
  ```sudo adduser adminserv
  cd /home/[username]
  mkdir .ssh
  vim .ssh/authorized_keys
  sudo chown adminserv:adminserv -R .ssh
  sudo usermod -aG sudo adminserv
  ssh -p 2803 adminserv@adminserv
  ```

  ## Intégration du projet et de la database :

   - ### Stack technique :
      VueJS, NodeJS + Postgres

  

<img width="1440" alt="Capture d’écran 2024-07-11 à 20 17 44" src="https://github.com/InesH2001/web_server_administration/assets/99474597/5dd1a989-2c64-4e81-b893-31f325415817">

<img width="1440" alt="Capture d’écran 2024-07-11 à 20 21 12" src="https://github.com/InesH2001/web_server_administration/assets/99474597/d405bec7-56bb-4812-82f7-a66a03fb98c5">

 ## Configuration de Ngnix : 

  - ### Création du fichier de conf d'NGNIX :
    - Default.conf
    - Ngnix.conf
    - Docker-compose.yml : (Configuration du Certbot et Samba dans le docker-compose)

 ## HTTPS : 

Configuration du serveur Nginx pour gérer les connexions HTTPS et pour proxy les requetes vers le frontend, le backend et Adminer

 ## Certbot : 
 Génération des certificats SSL 
  ``` docker run -it --rm \
  -v /etc/letsencrypt:/etc/letsencrypt \
  -v /var/lib/letsencrypt:/var/lib/letsencrypt \
  certbot/certbot certonly \
  --webroot -w /var/www/html \
  -d netmovies.store
 ```
 Vérification régulière (toutes les 12h) si les certificats doivent etre renouvelés
 
 Forcer le renouvelement du certificat: 
  ``` docker exec -it netmovies_certbot_1 certbot renew --dry-run  ```

<img width="1440" alt="Capture d’écran 2024-07-11 à 21 39 44" src="https://github.com/user-attachments/assets/8ca620a2-56b8-4b0c-80b2-0bdf1717f94f">

 ## Fail2ban : 


``` 
sudo nano /etc/fail2ban/jail.local
[nginx-http-auth]
enabled  = true
port     = http,https
filter   = nginx-http-auth
logpath  = /var/log/nginx/error.log
maxretry = 3

[nginx-botsearch]
enabled  = true
port     = http,https
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 2

[nginx-noscript]
enabled  = true
port     = http,https
filter   = nginx-noscript
logpath  = /var/log/nginx/error.log
maxretry = 6

[nginx-proxy]
enabled  = true
port     = http,https
filter   = nginx-proxy
logpath  = /var/log/nginx/error.log
maxretry = 0

[nginx-limit-req]
enabled  = true
port     = http,https
filter   = nginx-limit-req
logpath  = /var/log/nginx/error.log
maxretry = 5


[adminer]
enabled  = true
port     = http,https
filter   = adminer
logpath  = /var/log/php/adminer.log
maxretry = 3

sudo systemctl restart fail2ban
sudo fail2ban-client status
sudo systemctl status nginx 
```

## Execution Samba :

 Pour y accèder : 
  - Sur Windows : \IPV4\share
  - Sur Mac : smb://IPV4/share

Pour y acceder : 
Nom d'utilisateur : servuser 
Password : **********************
 


 
   

   
   




