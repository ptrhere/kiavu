# kiavu
streaming server config




# mettre en place un serveur de streaming

## access a un serveur 
hetzner (payer par heure !)

serveur dedie' 
- hetzner.com/sb  -- liste de enchères serveur dédié
- robot.hetzner.com

ou un VPS (virtual private server) 
- https://console.hetzner.cloud/
moins performant, mais ca peux q meme faire le taf, et c'est moins cher..


Utiliser l'interface web  d'Hetzner pour installer cette machine avec un linux 'Debian 12'

## ansible pour installer/configurer ce serveur

Une fois que hetzner nous donne access a une machine (mdp via email) et publie l'adresse IP (ipv4)
on est pret à la configurer avec un playbook 'ansible'

     apt install ansible 
     apt install sshpass  # needed for password based auth
   
     cd /mnt/d/.../myplaybook 
   
     ansible-playbook -i inventory install_vps.yml
   
   or with password auth:
   
     ansible-playbook -i inventory install_vps.yml -k
   
  
   
sur le workstation developeur

en meme temps (tant que c'est pas automatise' -- (*) ) on ajoute une entree DNS pour l'ip (afin de pouvoir generer un certificat par LetsEncrypt par la commande 'certbot')

     certbot  --nginx -d stream.l45.be
     
     nano /etc/nginx/nginx.conf 
     
     listen 443 ssl;
     
      systemctl restart nginx
      
      

(*) 
  https://docs.gandi.net/en/managing_an_organization/organizations/personal_access_token.html
  https://docs.ansible.com/ansible/latest/collections/community/general/gandi_livedns_module.html
  
  
a regarder: 
 - ajout du record dns automatiqe ?
 - ajouter l'authentication ? (OBS peux authentifier une connection stream)
 - interpreter les logs du nginx pour avoir une idee du nombre du public


# preparer OBS pour envoyer un stream live

pointer OBS sur rtmp://stream.L45.be/ingest
avec la cle' 'kievu'  (sans authentication)



# Interface video 
Sur le site de diffusion, intégrer la balise video et le sript JS :"hls.js" (*) ,
la source du serveur video est donné dans le script js intégré à la page.
Exemple : hls.loadSource('https://stream.l45.be/hls/kievu.m3u8');   

(*) https://github.com/video-dev/hls.js/



