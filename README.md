
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


Lancer la commande ./spluk start à partir du repertoire /opt/splunk/bin pour lancer l'installation:

	root@moabishells:/opt/splunk/bin# ./splunk start
	SPLUNK GENERAL TERMS

	Last Updated: August 12, 2021

	(...)
	SPLUNK GENERAL TERMS (August 2021)

	Do you agree with this license? [y/n]: y           

	This appears to be your first time running this version of Splunk.

	Splunk software must create an administrator account during startup. Otherwise, you cannot log in.
	Create credentials for the administrator account.
	Characters do not appear on the screen when you type in credentials.

	Please enter an administrator username: admin
	Password must contain at least:
	   * 8 total printable ASCII character(s).
	Please enter a new password: 
	Please confirm new password: 
	Copying '/opt/splunk/etc/openldap/ldap.conf.default' to '/opt/splunk/etc/openldap/ldap.conf'.
	Generating RSA private key, 2048 bit long modulus
	............+++++
	.............+++++
	e is 65537 (0x10001)
	writing RSA key

	Generating RSA private key, 2048 bit long modulus
	.................+++++
	....................................................+++++
	e is 65537 (0x10001)
	writing RSA key

	Moving '/opt/splunk/share/splunk/search_mrsparkle/modules.new' to '/opt/splunk/share/splunk/search_mrsparkle/modules'.

	Splunk> See your world.  Maybe wish you hadn't.

	Checking prerequisites...
		Checking http port [8000]: open
		Checking mgmt port [8089]: open
		Checking appserver port [127.0.0.1:8065]: open
		Checking kvstore port [8191]: open
		Checking configuration... Done.
			Creating: /opt/splunk/var/lib/splunk
			Creating: /opt/splunk/var/run/splunk
			Creating: /opt/splunk/var/run/splunk/appserver/i18n
			Creating: /opt/splunk/var/run/splunk/appserver/modules/static/css
			Creating: /opt/splunk/var/run/splunk/upload
			Creating: /opt/splunk/var/run/splunk/search_telemetry
			Creating: /opt/splunk/var/spool/splunk
			Creating: /opt/splunk/var/spool/dirmoncache
			Creating: /opt/splunk/var/lib/splunk/authDb
			Creating: /opt/splunk/var/lib/splunk/hashDb
	New certs have been generated in '/opt/splunk/etc/auth'.
		Checking critical directories...	Done
		Checking indexes...
			Validated: _audit _internal _introspection _metrics _metrics_rollup _telemetry _thefishbucket history main summary
		Done
		Checking filesystem compatibility...  Done
		Checking conf files for problems...
		Done
		Checking default conf files for edits...
		Validating installed files against hashes from '/opt/splunk/splunk-8.2.2.1-ae6821b7c64b-linux-2.6-x86_64-manifest'
		All installed files intact.
		Done
		Printing Splunk Cloud-only settings...
			Cloud-only setting "enableLogLevels (value :<boolean>)", defined in stanza [default] for spec "audit.conf.spec".
			Cloud-only setting "environment (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "iac.token.expiration (value :<integer>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "iac.url (value :<url>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "kvservice.auth.mode (value :external | vault | bridge)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "kvservice.namespace (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "kvservice.principal.client.id (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "kvservice.principal.client.secret (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "kvservice.principal.id (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "kvservice.principal.token (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "scs-kvstore-disabled (value :<boolean>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "scsTokenScriptPath (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
			Cloud-only setting "tenant (value :<string>)", defined in stanza [scs] for spec "/opt/splunk/etc/system/README/server.conf.spec".
	All preliminary checks passed.

	Starting splunk server daemon (splunkd)...  
	Generating a RSA private key
	................................................................+++++
	..........................+++++
	writing new private key to 'privKeySecure.pem'
	-----
	Signature ok
	subject=/CN=moabishells/O=SplunkUser
	Getting CA Private Key
	writing RSA key
	Done


	Waiting for web server at http://127.0.0.1:8000 to be available.............. Done


	If you get stuck, we're here to help.  
	Look for answers here: http://docs.splunk.com

	The Splunk web interface is at http://moabishells:8000

	root@moabishells:/opt/splunk/bin# 




Manuel: https://www.splunk.com/en_us/download/splunk-enterprise/thank-you-enterprise.html

## Configuration du forwarder

Configurer le forwarder pour envoyer les logs depuis /var/log/ vers votre instance de Splunk server Enterprise.

## Observation des logs

Un fois paramétrés, vos logs apparaissent dans la console de Splunk Enterprise.

Familiarisez vous avec cette console.

### J'ai fini plus tôt !

Good for you ! S'attacher à réaliser les wargames disponibles ici pour progresser: https://overthewire.org/wargames/


