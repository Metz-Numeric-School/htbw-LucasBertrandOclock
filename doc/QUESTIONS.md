# Questions

Répondez ici aux questions théoriques en détaillant un maxium vos réponses :

1. Expliquer la procédure pour réserver un nom de domaine chez OVH avec des captures d'écran (arrêtez-vous au paiement) :

   > - On comment par aller sur le site OVH (https://manager.eu.ovhcloud.com/#/hub/) et se connecter (ou créer un compte)
   > - Dans la barre de navigation, on choisi "WEB cloud" puis sur le bouton "Commander" on choisi nom de domaine
   > - Rechercher le nom souhaité, choisir l'extension, puis ajouter au panier
   > - Ajouter les options a notre nom de domaine (DNS, protection WHOIS, etc.) puis valider.
   > - Passer le paiement.
   >
   > ![ovh_barre_nav](/doc/screenshots/ovh_1.png)
   > ![ovh_domaine_navigation](/doc/screenshots/ovh_2.png)
   > ![ovh_domaine_selecteur](/doc/screenshots/ovh_3.png)
   > ![ovh_domaine_selected](/doc/screenshots/ovh_4.png)
   > ![ovh_domaine_option](/doc/screenshots/ovh_5.png)


2. Comment faire pour qu'un nom de domaine pointe vers une adresse IP spécifique ?

   > Aller dans la zone DNS du domaine (chez le registrar).  
   > Créer ou modifier un enregistrement A (IPv4) :
   >
   > - Nom : `@` (racine du domaine)
   > - Valeur : l'IP du serveur (ex: 203.0.113.10)
   > - TTL : par défaut
   > - (Option) Ajouter un enregistrement AAAA pour >l'IPv6.
   > - Attendre la propagation DNS (quelques minutes à >24h).

3. Comment mettre en place un certificat SSL ?
   > On commence par installer Certbot :  
   > `sudo apt -y install certbot python3-certbot-nginx`
   >
   > Obtenir le certificat :  
   > `sudo certbot --nginx -d example.com -d www.example.com`
   >
   > Certbot modifie la conf Nginx et active HTTPS.
   >
   > Renouvellement automatique :  
   > `sudo systemctl status certbot.timer`
