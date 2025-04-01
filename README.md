TrueNAS Scale

Sommaire
Étape 1 : Préparation des machines virtuelles (VM)
Installez un hyperviseur
Créez la première VM pour TrueNAS Scale
Créez la deuxième VM pour Debian
Étape 2 : Configuration de TrueNAS Scale
Connectez-vous à TrueNAS 
Créez le RAID 6 logiciel 
Étape 3 : Configuration de la VM Debian
Installez les outils réseau 
Vérifiez la connectivité 
Étape 4 : Connexions au serveur TrueNAS
Connectez-vous en HTTPS 
Activez les services SSH et FTP 
Créez des utilisateurs et dossiers 
Étape 5 : Test des connexions depuis Debian
Connexion SFTP avec FileZilla 
Connexion SSH 
Étape 6 : Configuration Samba et WebDAV
Activer Samba (partage de fichiers CIFS) 
Activer WebDAV
Étape 7 : Vérifications finales
Mises à jour et redémarrage 
Confirmez le changement de port SFTP/SSH 
Logiciels recommandés :
Hyperviseur : VirtualBox (gratuit).
Client SFTP : FileZilla (gratuit).
Client SSH : OpenSSH (intégré à Debian).
Introduction
TrueNAS SCALE est un système d'exploitation open-source basé sur Linux, conçu pour la gestion de stockage de données et la virtualisation. Il permet de créer des serveurs de stockage en réseau (NAS) et offre des fonctionnalités avancées telles que la gestion des volumes, la réplication des données, et l'intégration avec des solutions de conteneurs et de machines virtuelles. Il est particulièrement adapté aux entreprises et aux utilisateurs nécessitant une solution flexible et évolutive pour le stockage de données à grande échelle.
TrueNAS SCALE Plusieurs Avantages
Stockage centralisé et sécurisé : TrueNAS SCALE permet de centraliser le stockage des données, facilitant l'accès et la gestion des fichiers pour tous les utilisateurs du réseau. Il assure également la sécurité des données grâce à des fonctionnalités de cryptage et de sauvegarde.


Scalabilité : TrueNAS SCALE est conçu pour être évolutif. Vous pouvez facilement ajouter plus de stockage au fur et à mesure des besoins de votre entreprise, sans perturber le système en place.


Gestion simplifiée : Grâce à son interface web intuitive, TrueNAS SCALE permet une gestion facile des données, des utilisateurs et des volumes de stockage, même pour les personnes ayant peu d'expérience en gestion de serveur.


Réplication et sauvegarde : Il permet de mettre en place des sauvegardes régulières et des réplicas de données, réduisant ainsi le risque de perte de données en cas de panne matérielle ou d'incident.


Virtualisation et conteneurs : TrueNAS SCALE prend en charge les machines virtuelles et les conteneurs, ce qui permet d'exécuter diverses applications directement sur le système de stockage, offrant ainsi une grande flexibilité dans l'utilisation des ressources.


Open-source et gratuité : TrueNAS SCALE est un logiciel open-source, ce qui signifie qu'il n'y a pas de frais de licence. Cela en fait une solution économique, idéale pour les entreprises ayant un budget limité.


En résumé, TrueNAS SCALE nous offre une solution de stockage flexible, évolutive et sécurisée, tout en réduisant les coûts et en simplifiant la gestion des données dans votre bureau.












Étape 1 : Préparation des machines virtuelles (VM)
Installez un hyperviseur (VMware), à partir du lien officiel de téléchargement :
https://www.vmware.com/products/workstation-player.html




Créez la première VM pour TrueNAS Scale






















Les spécifications de la VM hébergeant le serveur doivent respecter les critères suivants : ➔ Processeur : 2 cœurs ➔ Mémoire vive (RAM) : 4 Go ➔ Disques durs (DD) : 2 de 16 Go ➔ Disques durs supplémentaires : 5 de 2 To, que vous convertirez en un espace de stockage de 2 Go en utilisant un RAID 6 logiciel. 








*










Créez la deuxième VM pour Debian
















Concernant la deuxième VM, l'installation de Debian avec une interface graphique est requise avec les critères suivants : ➔ Processeur : 2 cœurs ➔ Mémoire vive (RAM) : 2 Go ➔ Disques durs (DD) : 1 de 8 Go














































Étape 2 : Configuration de TrueNAS Scale
Connectez-vous à TrueNAS 
Démarrez la VM TrueNAS et suivez l’assistant d’installation (utilisez les 2 disques de 16 Go pour le système).
Notez l’adresse IP attribuée (ex: 192.168.124.128).



Créez le RAID 6 logiciel 

Accédez à l’interface web via https://[adresse-IP-truenas] ex: https://192.168.124.128
Allez dans Storage > Create Pool :
Nommez le pool "Stockage".
Sélectionnez les 5 disques de 2 To.
Choisissez RAID-Z2 (équivalent du RAID 6 dans TrueNAS).
Validez la création.




















Étape 3 : Configuration de la VM Debian
Installez les outils réseau 
Ouvrez un terminal et exécutez



















Vérifiez la connectivité 
            Testez la connexion à TrueNAS avec ping [192.68.124.128]




Étape 4 : Connexion au serveur TrueNAS
Connectez-vous en HTTPS 
Depuis Debian, ouvrir Firefox et accéder au lien: https(http)://192.168.56.129 depuis votre navigateur.
Identifiez-vous avec l’Id: truenas_admin et le mot de passe défini lors de l'installation.
         

Activez les services SSH et FTP 
Allez dans Services, activez SSH et FTP/SFTP.
Dans les paramètres SSH :
Modifiez le port par défaut (ex: 2222 au lieu de 22).
Sauvegardez et redémarrez le service.



























Créez des utilisateurs et dossiers 
Allez dans Credentials > Users > Add :
Créez des utilisateurs (ex: user1, user2).
Pour chaque utilisateur, créez un dossier dédié (ex: /mnt/Stockage/user1) et un dossier public (ex: /mnt/Stockage/public).















 














































Étape 5 : Test des connexions depuis Debian
Connexion SFTP avec FileZilla :
 Ouvrez FileZilla, utilisez :
Serveur : sftp://[adresse-IP-truenas].
Port : 2222 (ou celui configuré).
Identifiants : user1 + mot de passe.
Vérifiez l’accès aux dossiers dédiés et public.































Connexion SSH Dans le terminal 


Étape 6 : Configuration Samba et WebDAV
Configuration de Samba (partage de fichiers CIFS/SMB)
Activez le service SMB :
Accédez à l’interface TrueNAS SCALE.
Rendez-vous dans System Settings > Services.
Activez le service SMB.
Créer une nouvelle configuration de partage :
Allez dans Shares > SMB.
Cliquez sur Add pour créer un nouveau partage.
Spécifiez les informations suivantes :
Path : Chemin du dossier à partager (par exemple, /mnt/Stockage/dossierpublic).
Name : Nom du partage (par exemple, Public Share).
Configurez les permissions :
Autorisez l’accès en lecture/écriture pour les utilisateurs ou groupes spécifiques.
Désactivez les options d’accès anonyme si vous voulez une sécurité renforcée.
Test du partage Samba : Sur la machine Debian (VM 2), installez le client Samba si ce n’est pas déjà fait :
apt install smbclient
Connectez-vous au partage :
smbclient //<IP_du_serveur>/Public\ Share -U <nom_utilisateur>  smbclient //192.168.124.128/dossierpublic -U user1













 







Activation du service WebDAV :
Allez dans Applications - Discover
Installer WebDAV
Allez dans Applications - Installed pour vérifier que l’app a bien été installé et voir le statut de cette dernière
Allez dans Application Info - Edit 
Mettez en place la configuration de Webdav
Mettez en place la configuration des utilisateurs et des groupes
Mettez en place la configuration du réseau
Mettez en place la configuration de l’espace de stockage
Mettez en place la configuration des labels
Mettez en place la configuration des ressources
Procédez à la vérification et au test en accédant à WebDAV via un navigateur en tapant  http://IP_de_ton_serveur:port/ ou https://IP_de_ton_serveur:443/ selon la configuration













































Étape 7 : Vérifications finales
Mises à jour et redémarrage 
Dans TrueNAS, allez dans System > Update, vérifiez les mises à jour.
Redémarrez le serveur après les mises à jour.










Confirmez le changement de port SFTP/SSH :
Utilisez la commande netstat -tuln | grep 2222 sur TrueNAS pour vérifier que le port est actif.


 


















Logiciels recommandés :
Hyperviseur : VirtualBox (gratuit).
Client SFTP : FileZilla (gratuit).
Client SSH : OpenSSH (intégré à Debian).

Conclusion :
Ce projet a permis de déployer un serveur TrueNAS Scale fonctionnel avec :

Un stockage configuré en RAID 6 pour la redondance.

Des services sécurisés (HTTPS, SSH, SFTP).

Une gestion centralisée des utilisateurs et des permissions.

Annexes :
Commandes Utiles dans Bash:

Vérifier les disques
*lsblk

Vérifier le statut ZFS
*zpool status

Connexion SSH
*ssh user@ip -p port


Liens Utiles :

Documentation TrueNAS : https://www.truenas.com/docs/

Guide Debian : https://www.debian.org/doc/


