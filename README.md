
# CNAM TP6 : Agrégation des logs via Splunk

Le but de ces Travaux Pratiques est d'installer un agrégateur de logs sur le serveur Linux sécurisé implémenté durant le TP 3.

Nous allons utiliser un agrégateur commercial en version gratuite (Splunk Enterprise).

## Enregistrement aupres de Splunk

Visiter https://www.splunk.com/en_us/download/splunk-enterprise.html

Choisir en haut a droite "Splunk Gratuit" (ou "Free Splunk" si votre navigateur est en Anglais).

S'enregistrer et choisir la version de Splunk Enterprise. L'attente du mail de confirmation peut être un peu longue...

Une fois enregistré, vous disposez d'une version de démo de Splunk valide 60 jours.

## Comprendre le fonctionnement de Splunk

Regarder la vidéo: https://www.splunk.com/en_us/resources/videos/splunk-education-getting-data-in-with-forwarders.html

## Installer le serveur splunk

Depuis la page https://www.splunk.com/en_us/download/splunk-enterprise.html
Telecharger la version du serveur correspondant à votre host (Linux ou Windows).

Exemple d'installation .deb sous Linux:

	root@moabishells:/tmp# dpkg -i ./splunk-8.2.2.1-ae6821b7c64b-linux-2.6-amd64.deb 
	Selecting previously unselected package splunk.
	(Reading database ... 164465 files and directories currently installed.)
	Preparing to unpack .../splunk-8.2.2.1-ae6821b7c64b-linux-2.6-amd64.deb ...
	Unpacking splunk (8.2.2.1) ...
	Setting up splunk (8.2.2.1) ...
	complete
	root@moabishells:/tmp#


## Telecharger le forwarder

Le but du forwarder est de répliquer les logs locaux de votre Linux sécurisé sur le serveur Splunk. Télécharger le forwarder depuis l'adresse https://www.splunk.com/en_us/download/universal-forwarder.html (choisir la version .deb Linux 64bits Intel).

Installer le forwarder sur votre serveur Linux sécurisée.

	jonathan@blackbox:~/CNAM/tp6$ scp ./splunkforwarder-8.2.2.1-ae6821b7c64b-linux-2.6-amd64.deb root@XXXXXXX:/tmp/
	splunkforwarder-8.2.2.1-ae6821b7c64b-linux-2. 100%   25MB   4.0MB/s   00:06    
	jonathan@blackbox:~/CNAM/tp6$ 

Sur le serveur:

	root@moabishells:~# dpkg -i /tmp/splunkforwarder-8.2.2.1-ae6821b7c64b-linux-2.6-amd64.deb 
	Selecting previously unselected package splunkforwarder.
	(Reading database ... 163883 files and directories currently installed.)
	Preparing to unpack .../splunkforwarder-8.2.2.1-ae6821b7c64b-linux-2.6-amd64.deb ...
	Unpacking splunkforwarder (8.2.2.1) ...
	Setting up splunkforwarder (8.2.2.1) ...
	complete
	root@moabishells:~# 

Note: La commande "splunk" décrite dans la documentation est présente sous /opt/splunkforwarder/bin/splunk. Par defaut, elle n'est pas executable : il faut donc changer ses permissions.

	root@moabishells:~# chmod +x /opt/splunkforwarder/bin/
	root@moabishells:~# /opt/splunkforwarder/bin/splunk
	Data forwarding configuration management tools.
	  Commands:
	      enable local-index [-parameter <value>] ...
	      disable local-index [-parameter <value>] ...
	      display local-index
	      add forward-server server
	      remove forward-server server
	      list forward-server
	  Objects:
	      forward-server       a Splunk forwarder to forward data to be indexed
	      local-index          a local search index on the Splunk server
	root@moabishells:~# 


Manuel: https://www.splunk.com/en_us/download/splunk-enterprise/thank-you-enterprise.html

## Configuration du forwarder

Configurer le forwarder pour envoyer les logs depuis /var/log/ vers votre instance de Splunk server Enterprise.

## Observation des logs

Un fois paramétrés, vos logs apparaissent dans la console de Splunk Enterprise.

Familiarisez vous avec cette console.

### J'ai fini plus tôt !

Good for you ! S'attacher à réaliser les wargames disponibles ici pour progresser: https://overthewire.org/wargames/


