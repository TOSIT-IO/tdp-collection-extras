LDAP - Kerberos
===============

Ce role installe un annuaire OpenLDAP et un serveur MIT Kerberos utilisant le backend LDAP.
Il permet également d'installer la configuration cliente.

Serveur
-------

LDAP
^^^^

Un service ldap (slapd) est instancié. La configuration initiale est gérée de manière statique dans le fichier `slapd.conf`. Cela implique qu'une modification de la configuration nécessite un redémarrage du service slapd. Cette configuration et le redémarrage si nécessaire sont gérés par le role. 

Une fois le service slapd opérationnel, un arbre de base y est ajouté, cette fois-ci de manière dynamique avec un fichier `ldif` et la commande `ldapmodify`. C'est ce qui doit être fait pour toute les futures modifictions comme des ajouts d'utilisateurs dans le ldap.

Dans cette arbre de base (exemple: `c=io`), différentes branches peuvent être ajoutées comme par example une branche d'utilisateurs. Le role ne gère actuellement pas la gestion de ces branches.

Kerberos
^^^^^^^^

Un serveur Kerberos est installé et sa configuration (serveur) est contenue dans le fichier `kdc.conf` (cf templates). MIT Kerberos peut gérer différentes bases de données en backend pour stocker ces principaux. Ce role configure le backend ldap, se reposant ainsi sur le LDAP installé au préalable.

A terme, l'intégration avec le LDAP permet d'avoir un setup Kerberos en HA efficace et assez simple à mettre en place, en basant sur les mechanismes de réplication de LDAP. Le role ne gère pas encore la haute disponibilité. Le backend ldap permet également la création d'alias de principaux (cf. derniers paragraphes https://web.mit.edu/kerberos/krb5-devel/doc/admin/conf_ldap.html).

Un pricipale d'administration est créé, par défaut `admin/admin`. Il permet de se connecter au service `kadmin` qui tourne sur le serveur. Ce service `kadmin` permet d'effectuer toutes les opération d'administrations Kerberos, comme l'ajout des principaux.

Client
------

Ce role configure les clients pour permettre l'authentification et la récupération de tickets Kerberos. La configuration Kerberos au niveau des machines clientes se situe dans `/etc/krb5.conf`. Les binaires utiles sont installés. 

Example d'utilisation: 

1. Authentification: commande `kinit admin/admin`
2. Démarrage du shell d'aminstration: commande `kadmin` (nécessite de s'être authentifié au préalable avec un compte d'administration, cf 1))
3. Liste des principaux Kerberos: commande `listprinc` dans le shell kadmin.

