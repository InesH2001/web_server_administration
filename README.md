
# Nom de domaine en HTTPS: 

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

## Configuration du serveur : 

- ### Ajouter un nouveau user :
    ```sudo adduser adminserv ```

- ### Ajouter l'utilisateur au groupe sudo :
  ```sudo usermod -aG sudo adminserv ```
  
- ### Create SSH Key :
 
   Génération la clé SSH
   ``` ssh-keygen -t rsa -b 4096 -C "notre e-mail" ```

   CP clé publique
   ``` cat ~/.ssh/id_rsa.pub ```

   Se connecter au serveur Digital Ocean :
   ``` ssh -p 2803 adminserv@IPV4 ```

   Ajouter la clé SSH au fichier 'authorized_keys' :
   ``` echo "clé_publique_copiée" >> ~/.ssh/authorized_keys ```

   Attribuer les bonnes permissions : ``` sudo chmod 700 /home/new_username/.ssh ```
   ``` sudo chmod 600 /home/new_username/.ssh/authorized_keys```
   ``` sudo chown -R new_username:new_username /home/new_username/.ssh ```

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


 ## Excution Samba : 
 Pour y accèder : 
  - Sur Windows : \IPV4\share
  - Sur Mac : smb://IPV4/share

Pour y acceder : 
Nom d'utilisateur : srvuser 
Password : **********************
 


 
   

   
   




