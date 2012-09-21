# Installation de NodeJS sur Debian Lenny

1. Télécharger le package depuis http://nodejs.org/dist/ par exemple avec la commande : wget http://nodejs.org/dist/v0.8.8/node-v0.8.8.tar.gz
1. Détarer le package : tar xzvf node-v0.8.8.tar.gz
1. Aller dans le dossier : cd node-v0.8.8
1. configurer les sources en vue de la compilation : ./configure
1. Compiler : make
1. Installer (par défaut dans /usr/local/ sauf si utilisation du paramètre --prefix lors du ./configure) : make install 
1. Aller dans un autre dossier : cd ..
1. Vérifier le fonctionnement de node : node -v
1. Le numéro de version de node doit s'afficher, soit : v0.8.8



## Erreur Python : import json ImportError: No module named json
Si cette erreur se produit (normalement lors du make install), c'est que python à besoin du package json, mais qu'il n'est pas installé.
Celui-ci a été introduit dans python en version 2.6.
Sur Debian Lenny, la version de python est 2.5.
Il est possible d'utiliser simplejson (qui a la même interface que json).

Pour cela :
1. Récupérer les sources de simplejson ici : http://pypi.python.org/pypi/simplejson
1. Par exemple : wget http://pypi.python.org/packages/source/s/simplejson/simplejson-2.6.1.tar.gz#md5=03fbb6c4a4d37a767af7848f902e2ad4
1. Détarer le package : tar xzvf simplejson-2.6.1.tar.gz
1. Aller dans le dossier : cd simplejson-2.6.1
1. Construire le package : python setup.py build
1. Installer le package : python setup.py install


Il faut ensuite modifier le fichier python pour utiliser simplejson et relancer ensuite l'installation de nodejs via les commandes : 
1. cd node-v0.8.8
1. Editer le fichier tools/install.py : vi tools/install.py
1. Modifier le fichier en remplaçant la ligne en erreur ("import json" ligne 4) par les lignes suivantes :
    try:
        import json
    except ImportError:
        import simplejson as json
1. Relancer l'installation de nodejs : make install
1. Aller dans un autre dossier : cd ..
1. Vérifier le fonctionnement de node : node -v
1. Le numéro de version de node doit s'afficher, soit : v0.8.8


## Erreur lors du lancement de node : version `GLIBC_2.9' not found (required by node)

Cette erreur a été observé lors de l'utilisation des binaires pré-compilé Linux téléchargeable depuis le site de NodeJS.
En effet, sur Debian Lenny, la version de libc et la version 2.7.18 au mieux.
Essayer l'installation détaillée de début de cette document, elle fonctionne sur Debian Lenny, même en ayant la version 2.7.18 de libc.