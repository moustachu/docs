# Democracy OS : déployer une instance en ligne

Democracy OS est une application web permettant la saisie, le débat et le vote de proposition présentée sous forme d’article.

[Site officiel](https://www.democracyos.org) (en anglais)  
[Site de développement](github.com/democracyos/app) (en anglais)  

Democracy OS est écrit en Javascript / HTML / CSS et nécessite la plateforme logicielle [Node.js](https://nodejs.org/) ([wikipedia](http://fr.wikipedia.org/wiki/Node.js)) ainsi qu'une base de données [MongoDB](http://www.mongodb.org/) ([wikipedia](http://fr.wikipedia.org/wiki/MongoDB)).  
*A titre de comparaison, Wordpress est écrit en PHP / Javascript / HTML / CSS et nécessite l’environnement PHP et une base de donnée MySQL.*

Ce tutoriel a pour objectif de présenter une méthode simple de déploiement de Democracy OS en ligne pour une plateforme de test :
+ hébergement gratuit chez [heroku.com](http://www.heroku.com/) (en anglais) (une carte bancaire valide sera néanmoins nécessaire)
+ configuration et déploiement intégralement en ligne
+ protection optionnelle du site par login / mot de passe

**Attention !** Si il est possible de partir de cette plateforme de test pour en faire une plateforme de production, il est préférable de considérer en premier lieu le cout de l’hébergement. Heroku ([wikipedia](http://fr.wikipedia.org/wiki/Heroku)) est un service de type plateforme en tant que service (PaaS), très aimé par les développeurs pour sa simplicité d’utilisation, son intégration avec d’autres services web et sa riche documentation en ligne.  
Néanmoins, comme toute bonne chose, devenir dépendant de ce service peut finir par couter cher car son mode de facturation est basé sur la consommation et la puissance demandées (utilisateurs simultanés, espace de stockage, messages reçus et envoyés…).
*→ faire une liste des offres serveurs type Gandi et OVH*

Pour savoir comment installer Democracy OS en détail, notamment dans le cas d’une personnalisation étendue ou d’une mise en production, il est préférable de consulter la [documentation officielle de l’application](https://github.com/DemocracyOS/app/wiki/Installation) (en anglais).

## 1 - Configurer son hébergement

[**Créer un compte sur heroku**](https://signup.heroku.com/www-home-top)
( ou [utilisez un compte existant](https://id.heroku.com/login) )


Le compte doit être validé par une adresse email valide et un numéro valide de carte bancaire.  
Ceci va vous permettre d’activer les add-ons gratuits nécessaires au fonctionnement de Democracy OS.  
+ [gestion du compte heroku](https://dashboard.heroku.com/account)
+ [coordonnées de facturation](https://dashboard.heroku.com/account/billing)

[**Créer une nouvelle application sur heroku**](https://dashboard.heroku.com/new)

[**Ajouter l’add-on MongoLab à l’application**](https://addons.heroku.com/mongolab) (Offre Sandbox Free)


## 2 - Récupérer le code de Democracy OS

[**Créer un compte sur github**](https://github.com/join)
( ou [utilisez un compte existant](https://github.com/login) )

*Un compte gratuit est suffisant, ses projets seront publics mais ne contiennent pas d’informations privées.*

[**« Forker » le code de Democracy OS**](https://github.com/DemocracyOS/app) ( ie. en faire une copie personnelle )  
+ Allez sur la [page](https://github.com/DemocracyOS/app) du code de l'application Democracy OS
+ Créer une copie du code à l’aide du bouton « **Fork** » (en haut à droite)
    ![github fork][img01]
+ Et c’est tout ...


# 3 - Configurer et lancer Democracy OS

[**Ouvrir les paramètres de votre application heroku**](https://dashboard.heroku.com/apps)  

    https://dashboard.heroku.com/apps/<nom-application-heroku>/settings  

*où __ < nom-application-heroku > __ est le nom de l'application créée sur heroku*

### Ajouter les variables de configuration de Democracy OS

C’est la partie pénible du tutoriel avec beaucoup de copier / coller...  
Ne pas hésiter à sauvegarder les variables de configuration le plus souvent possible pour éviter de perdre son travail.

+ Dans la section « **Config variables** » en cliquant sur le bouton « **Reveal Config Vars** »
   ![config vars][img02]
+ Puis le bouton « **Edit** » pour activer le mode édition
  ![config vars][img03]
+ Et enfin « **+** » pour ajouter des variable de configuration
  ![config vars][img04]
+ Répéter le processus pour sauvegarder le plus souvent possible.

Les variables à configurer sont les suivantes :


**NODE_ENV**

    production


**NODE_PATH**

     .


**GITHUB_USERNAME**  
*identifiant github*


**GITHUB_PASSWORD**  
*mot de passe github*


**HOST**  
*adresse de l’application heroku*

    <nom-application-heroku>.herokuapp.com


**PROTOCOL**

    80


**LOCALE**  
pour français

    fr  

pour anglais

    en

les langues disponibles sont consultables [ici](https://github.com/DemocracyOS/app/tree/master/lib/translations/lib)


**MONGO_URL**  
*adresse de connection à la base MongoDB*

    mongodb://...

Normalement, l’add-on MongoLab créé une variable nommée MONGOLAB_URI avec l’adresse complète


**MONGO_USERS_URL** *optionnel*  
*adresse de connection à la base MongoDB pour les utilisateurs*

    mongodb://...

Cette adresse peut être la même que la base principale.


**NOTIFICATIONS_URL**  
*adresse de votre service de notification si existant*

    null

Cette variable doit néanmoins être définie avec une valeur


**NOTIFICATIONS_TOKEN**  
*clé de connexion de votre service de notification*

     1234

Cette variable doit néanmoins être définie avec une valeur


**STAFF**  
*adresse(s) email des utilisateurs autorisés à administrer l’application Democracy OS*

    <email> ( , <email> … )


**FAVICON**  
*adresse de l’icône du site (URL externe possible)*

    /lib/boot/images/favicon.ico


**LOGO**  
*adresse du logo du site (URL externe possible)*

    /lib/boot/images/logo.png


**ORGANIZATION_NAME**  
*nom de l'organisation*


**ORGANIZATION_URL**  
*adresse de l'organisation*

    http://...


**RSS_ENABLED**
*activation des flux RSS*

    true ou false


**COMMENTS_PER_PAGE**  
*nombre de commentaire par page*

    10


**JWT_SECRET**  
*n’importe quel texte (utilisé pour le cryptage)*

     just a secret phrase


**BASIC_USERNAME** *optionnel*

     <identifiant>

Pour protéger intégralement le site avec un unique couple identifiant / mot de passe avec [BasicAuth](http://en.wikipedia.org/wiki/Basic_access_authentication) (en anglais)


**BASIC_PASSWORD** *optionnel*

    <mot-de-passe>

Pour protéger intégralement le site avec un unique couple identifiant / mot de passe avec [BasicAuth](http://en.wikipedia.org/wiki/Basic_access_authentication) (en anglais)


[Pour plus d’informations sur les variables de configuration de Democracy OS](https://github.com/DemocracyOS/app/wiki/Environment-variables) (en anglais)


### Déployer l’application Democracy OS sur heroku


**Aller dans l’onglet « **Deploy** » de l'application**

    https://dashboard.heroku.com/apps/<nom-application-heroku>/deploy/heroku-git

*où < nom-application-heroku > est le nom-de l’application créée sur heroku*

**Connecter le compte github qui contient la copie de Democracy OS  
et autoriser heroku à voir les dossiers hébergés sur github**

![config vars][img05]  
![config vars][img06]

**Sélectionner votre copie de Democracy OS**

![config vars][img07]

**Déployer la branche master**

![config vars][img08]

*__en cas d’échec, ne pas hésiter à recommencer le déploiement une ou 2 fois__*

**en cas de succès, l’application est disponible à l’adresse**

    https://<nom-application-heroku>.herokuapp.com


Et voilà !


## 4 - Pour aller plus loin

L’instance obtenue est idéale pour une phase de test, ou de démarrage d’un projet avec peu de ressources.  
Néanmoins, pour avoir une instance complètement opérationnelle il est nécessaire d’adresser les points suivants :
+ Configuration du service de notification par email de DemocracyOS
+ Personnalisation de l’apparence de l'application
+ Ajout des pages légales, d’aide et lexique
+ Connexion d’un nom de domaine existant (autre que xxx.herokuapp.com)
+ et Obtention d’un certificat SSL pour la connexion sécurisée

Il est préférable d’avoir un petit bagage technique pour pouvoir finaliser un tel déploiement.  
De plus, pour une meilleure disponibilité de l'application, il faut envisager le cout de son hébergement.

A titre d’exemple, le service de notification de Democracy OS nécessite une autre instance Node.Js pour fonctionner en permanence.  
Or avec l’offre gratuite de heroku, une seule instance Node.js est mise à disposition et elle est déjà utilisée par l’application Democracy OS.  
Il faut louer les services au minimum d’une autre instance Node.Js et payer la facture en fin de mois.

A partir de ce point, la majorité des liens vont pointés vers des pages en anglais.

[**Consulter le wiki de développement de Democracy OS**](https://github.com/DemocracyOS/app/wiki) (en anglais)

**Apprendre à utiliser heroku**

Heroku est une plateforme très accessible pour les novices du domaine.  
Son utilisation répandue fait qu’elle est largement documentée et facilite l’apprentissage des technologies sous-jacentes.
[Tutoriel Node.Js sur heroku](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction) (en anglais)

**Configurer son service de notification Democracy OS**

[page du service de notification](https://github.com/DemocracyOS/notifier) (github)

En suivant la même procédure que pour l’application principale, il est possible de déployer le service de notification de Democracy OS.  
Soit en achetant une nouvelle instance Node.Js, soit en ouvrant un autre compte heroku avec une adresse email.  
Il est possible d’utiliser la même carte bancaire que pour le compte principal afin d’activer les add-ons suivants :
+ [Mandrill](https://addons.heroku.com/mandrill) pour envoyer des emails (offre Starter Free)
+ [Logentries](https://addons.heroku.com/logentries) pour les logs systèmes (offre TryIt Free)

*Mandrill est un service fourni par [Mailchimp](http://mailchimp.com/) et offre 12000 emails gratuits par mois (et 250 maximum par heure).  
Comme pour MongoLab, ses 2 add-ons fournissent automatiquement les identifiants de connexion demandé pour Democracy OS.*

**Editer et compiler son code en ligne** (pour les extrémistes du mode en ligne)
+ Stocker votre code sur [github](https://github.com/)
+ Stocker vos fichiers privés sur [dropbox](https://www.dropbox.com/fr/)
+ Editer et compiler votre code sur [cloud9](https://c9.io/) ou [koding](koding)
+ et bien sur connecter les services entre eux
+ *et oui, ça marche sur téléphone et tablette ...*

[img01]: ./heroku-sandbox-101-resources/image-01.jpg
[img02]: ./heroku-sandbox-101-resources/image-02.jpg
[img03]: ./heroku-sandbox-101-resources/image-03.jpg
[img04]: ./heroku-sandbox-101-resources/image-04.jpg
[img05]: ./heroku-sandbox-101-resources/image-05.jpg
[img06]: ./heroku-sandbox-101-resources/image-06.jpg
[img07]: ./heroku-sandbox-101-resources/image-07.jpg
[img08]: ./heroku-sandbox-101-resources/image-08.jpg
