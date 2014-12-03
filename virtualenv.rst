Pour un Projet
==============
Tout est dit de ici:
http://docs.python-guide.org/en/latest/dev/virtualenvs/#basic-usage
à là:
http://docs.python-guide.org/en/latest/dev/virtualenvs/#other-notes

Installation
------------
# Avec '--no-site-packages' si virtualenv < 1.7 ; sinon c'est déjà le 'default'
$ virtualenv -p /usr/bin/python2.7 venv 

Ensuite:
$ source venv/bin/activate

$ pip install -r requirements.txt

Requirements
------------
Si après quelque temps on a du ajouter des libs, 
AVEC ce genre de virtualenv (!!!)
il suffira de faire:

$ pip freeze > requirements.txt

Pour pouvoir commiter le changement de dépendances.
Il ne reste qu'à faire un lien vers ce fichier dans le setup.py.
Qui servira à générer la documentation (!!!)

Pour le Système
===============
La bonne version
----------------
Il semble qu'il faille dès le départ se méfier de la version qui sera installée.
Il y a tout une portion de la doc consacrée aux recommandations en terme de versions minimales.
http://virtualenv.readthedocs.org/en/latest/virtualenv.html#installation

Pour les sites webs en mod-wsgi
-------------------------------
Il y a éventuellement des trucvs à trifouiller dedans. 
À relire quand ça fera un peu de sens:
http://virtualenv.readthedocs.org/en/latest/virtualenv.html#using-virtualenv-without-bin-python
