
# ADCS : installer un Certificat Webserver dans IIS
 /!\ Remplacer les différentes informations par les votres (IE: nom,domaine, FQDN) /!\

## 1ère étape : Génération de la requête de certificat
***Sur le serveur IIS***

Dans la console de gestion de IIS, ouvrir la console *Certificat*, faire un clic-droit et sélectionner *Créer une demande de certificat*
<img src= "https://i.imgur.com/j08CUG5.jpg">

Renseigner les informations suivantes.
<img src= "https://i.imgur.com/Ww1JQbj.jpg">

Sélectionner le paramétrage ci-dessous.
<img src= "https://i.imgur.com/evZJJJU.jpg">

Renseigner le dossier et de nom où se trouvera la requête.
<img src= "https://i.imgur.com/cJfkZ0r.jpg">
<img src= "https://i.imgur.com/Iwxd3Yv.jpg">


## 2ème étape : Récupération de la requête
***Sur le serveur AD-CS***  
***Via Partage réseau / Winscp / VMtools / Copier-Coller***

## 3ème étape : Création du certificat
***Sur le serveur AD-CS***

Sur PowerShell, entrer les commandes suivantes :
`certutil -setreg policy\EditFlags +EDITF_ATTRIBUTESUBJECTALTNAME2`  
ET  
<img src= "https://i.imgur.com/q3iS8ia.jpg">
`certreq -submit -attrib "CertificateTemplate:WebServer\nSAN:dns=Ton_FQDN&ipaddress=IP_IIS" Requête.txt`

Sélectionner l'autorité de certification.
<img src= "https://i.imgur.com/K5QY9Xo.jpg">

Enregistrer le certificat en .cer
<img src= "https://i.imgur.com/GnMbzLq.jpg">

## 4ème étape : Récupération du certificat
***Sur le serveur IIS***  
***Partage réseau / Winscp / VMtools / Copier/Coller***

## 5ème étape : Importation du certificat
***Sur le serveur IIS***

Dans la console de gestion de IIS, ouvrir la console *Certificat*, faite un clic-droit et sélectionner *Terminer une demande de certificat*.
<img src= "https://i.imgur.com/KOdtAEL.jpg">

Sélectionner le certificat.
<img src= "https://i.imgur.com/IVGFZh1.jpg">

## 6ème étape : Débugage certificat fantôme
***Sur le serveur IIS***  
(Si le certificat disparait dès que la console est actualisée, suivez les étapes suivantes)

Dans la console de certificat, dans le magasin *Hébergement Web*, rechercher le numéro de série de votre certificat et le copier.
<img src= "https://i.imgur.com/zwFlCEF.jpg">

Sous PowerShell, entrer la commande suivante :  
`certutil -repairstore webHosting Serial_Number
`
Résultat :  
<img src= "https://i.imgur.com/ypdlIzS.jpg">

Rafraichir la console IIS et le certificat devrait réapparaître.

## 7ème étape : Liaison HTTPS
***Sur le serveur IIS***

Dans la console de gestion de IIS, faire un clic-droit sur le VHost(liaison) et sélectionner *Modifier les liaisons*.
<img src= "https://i.imgur.com/4Gy57qT.jpg">

Créer la liaison suivante en sélectionnant le certificat récemment importé.
<img src= "https://i.imgur.com/OyHEEj4.jpg">

## 8ème étape : Redirection HTTP => HTTPS

Superbe procédure :
https://www.ssl.com/how-to/redirect-http-to-https-with-windows-iis-10/

Suivant les versions de Windows Server le HSTS peut déjà exister en option, sinon il faut installer l'équivalent du mod-rewrite d'apache
***

### ENJOY
**Merci à l'intelligence collective du groupe 37_AIS2023_1 et à Nathan**

-----
Dernière MAJ : 26/10/2023 - CEFIM - groupe 37_AIS2023_1