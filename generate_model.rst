Résumé:
=======

Les paquets à installer:

    # apt-get install dia libparse-dia-sql-perl
    
Création du diagramme: dia
--------------------------

La construction du diagramme se fait d'après la même méthode que pour tedia2sql [left.subtree].
NB: Les id doivent être "protected" pour que ça compile, etc...

Attention pour les "Component", au moment d'écrire les INSERT rien ne sert de s'acharner à double cliquer partout.
Il faut cliquer sur l'icône pour écrire. 
De même pour coller il faut l'icône ou le menu édition, mais c'est possible.

La seule partie du tutoriel qui n'a pas marché, c'est les Typemaps.
Il est possible de les écrire mais il ne sont pas reconnus par "parsediasql" au moment de la compilation.
Ce sera indiqué dans le fichier .sql

Il reste donc à corriger ce point.

Generation du SQL: parsediasql
------------------------------

C'est par la ligne de commande avec "parsediasql":
    
    $ parsediasql --file  model.dia --db mysql-innodb
    
    $ parsediasql --file  model.dia --db mysql-innodb 1>/dev/null
    
    $ parsediasql --file model.dia --db mysql-innodb 2>/dev/null 1>schema.sql
    
    $ geany schema.sql 
    
Vérification du SQL: mysql ou sqlite
------------------------------------

    $ mysql -u root -p
    
    mysql> create database test_dia;
    
    mysql> show databases;
    
    mysql> use test_dia
    
    mysql> source schema.sql
    
    mysql> show tables;
    
    mysql> \u mysql

    mysql> drop database test_dia;
 
Generation du SQL à partir d'un Diagramme:
==========================================
Finalement il a été retenu d'utiliser:
* "dia" pour créer le diagramme
* "parsediasql" pour générer le sql correspondant
* SGBD: "mysql"

Génération du SQL avec "tedia2sql": Prbl...
----------------------------------- 
Il y a un tutoriel bien pratique pour générer les tables sql avec Dia [sql_dia].
Le diagramme est crée comme un diagramme de classe UML habituel, avec les attributs pour les données.
Il est expliqué comment faire le lien d'aggrégation pour la clé étrangère.
Ensuite il y a la commande pour générer le sql à partir du fichier:

    tedia2sql -i model.dia -o schema.sql -t innodb -f

Il y a d'autres options indiquées pour faire d'autres SGBD. 
Malheureusement l'outil plante sur ubuntu 12.04, que ce soit un fichier .dia compressé ou non, et rien n'est indiqué sur la page du projet [tedia2sql]. 
Sur le wiki de gnome [wiki.gnome] il y a toute une liste d'autres outils suceptibles d'aider à faire du sql, par exemple 
[dia2code] qui n'a pas l'air très folichon avec ses GUI bizarres. 
[diacenter] est un outil construit par des allemands basé sur les autres scripts. 
Attention il nécessite python-tk. 
Et donne quand même une erreur, apparemment notre version de python-tk est trop récente il lui manque /usr/lib/python2.7/lib-tk/Tix.py.

Generation du SQL avec "umbrello": Prbl...
----------------------------------
Un autre essai de génération de sql a été fait avec [umbrello]. 
Il ne crée que les tables sans les attributs !!!
Ce problème existait pour les versions de [2008], 
mais notre version actuelle est bien supérieure (Version 2.8.5, KDE Version 4.8.5).
Un essai avec "tedia2sql" crée bien le fichier "schema.sql" à partir de ".xmi". 
Mais le résultat ne contient ni tables ni attributs!
Apparemment le format .xmi poserait pas mal de problèmes en fonction des versions d'UML et des logiciels [interopératibilité].
D'après cette précédente page, le meilleur générateur serait [AndroMDA].

Génération du SQL avec "AndroMDA": Prbl...
--------------------------------
C'est un projet en licence BSD utilisable pour tout OS. 
Pour l'installer il faut aussi avoir maven2 [Build AndroMDA] 
    
    ~/andromda-bin-3.3 $ mvn install -Pandromda-full,local

Attention, il y a sur leur explication une commande à passer, mais elle ne passe pas das la console:

    ~/andromda-bin-3.3 $ MAVEN_OPTS=-XX:MaxPermSize=256m -Xmx512m
    
Problème de version/éditeur de Java:

    genevieve@genevieve-Studio-1535 ~/andromda-bin-3.3 $ mvn install -Pandromda-full,local

    [INFO] Scanning for projects...

    [WARNING] 

    	Profile with id: 'andromda-full' has not been activated.
   
    [WARNING] 

    	Profile with id: 'local' has not been activated.
    
    [INFO] ------------------------------------------------------------------------

    [INFO] Building Maven Default Project

    [INFO]    task-segment: [install]

    [INFO] ------------------------------------------------------------------------

    [INFO] ------------------------------------------------------------------------

    [ERROR] BUILD ERROR

    [INFO] ------------------------------------------------------------------------

    [INFO] Cannot execute mojo: resources. It requires a project with an existing pom.xml, but the build is not using one.

    [INFO] ------------------------------------------------------------------------

    [INFO] For more information, run Maven with the -e switch

    [INFO] ------------------------------------------------------------------------

    [INFO] Total time: 1 second

    [INFO] Finished at: Fri Jan 24 06:22:57 CET 2014

    [INFO] Final Memory: 4M/53M

    [INFO] ------------------------------------------------------------------------

    
L'erreur viendrait d'un problème de version de java pour cette erreur [mojo] 
En réponse ils renvoie vers une solution sur stackoverflow, à essayer [maven - openjdk].

Génération du SQL avec "parsediasql": OK!!!
-------------------------------------------
En fait c'était précisé dans un cadre sur la référence de tedia2sql, celui ci ne marche plus à partir de Dia 0.97! [tedia2sql]
Ils renvoient vers leur site [Parse-Dia-SQL].

    # apt-get install  libparse-dia-sql-perl
    
    $ parsediasql --file  model.dia --db mysql-innodb

On obtient dans la sortie standard le fichier .sql généré.
Cela permet déjà d'affiner le diagramme.
La manière plus évoluée, qui permet d'avoir entre autre les clés étrangères et bien plus, est décrite pour tedia2sql [left.subtree].

Conversion SQL vers "model.py":
===============================

Avec "mysql" et "sqlautocode"

Pypi sqlautocode: Installation
------------------------------
Apparemment le module "sqlautocode" n'est plus très maintenu.

Il n'est pas présent dans les dépots, mais sur plusieurs sites d'où il peut être téléchargé:

    https://code.google.com/p/sqlautocode/

    https://pypi.python.org/pypi/sqlautocode

Pour l'installation, ni "sudo easy install ni pip" n'ont voulu marcher, mais par contre la solution "bourrine" marche:
~/sqlautocode-0.7 $ sudo python setup.py install

Attention il faut prendre la version 0.7 !

Pypi sqlautocode: avec Mysql
----------------------------
NB: Il faut au préalable créer la base de données "test_dia".

    ~/ $ sqlautocode mysql://root@localhost/test_dia 

    ~/ $ sqlautocode mysql://root@localhost/test_dia -o model.py

C'est la bonne ligne de commande, celle de googlecode ne marche plus et renvoie des erreurs, et il faut absolument mettre l'objet à produire APRES la phrase de connection.

Il va falloir fouiller un peu dans les options.

Après vérification ça marche en gardant les clés étrangères et les inserts:

Apparemment ce n'est pas grave pour le latin1 dans le fichier SQL, car sqlautocode va de toute façon convertir par défaut en utf8 (cf options --help).

Mais quoi qu'il en soit il faudra revenir plus tard sur ce problème de l'encodage avec mysql.

Edition du model.py brut (mysql):
---------------------------------


References:
===========

References Diagramme => SQL:
----------------------------

[sql_dia] http://www.coderholic.com/automatic-sql-generation-using-dia/

[tedia2sql] http://tedia2sql.tigris.org/

[wiki.gnome] https://wiki.gnome.org/Apps/Dia/Links

[dia2code] http://dia2code.sourceforge.net/gui.html

[diacenter] http://diacenter.wspiegel.de/

[umbrello] http://docs.kde.org/development/en/kdesdk/umbrello/code-import-generation.html

[2008] http://osdir.com/ml/linux.umbrello.user/2007-08/msg00001.html

[interopératibilité] http://jmvanel.free.fr/uml/uml-interop.html

[AndroMDA] http://www.andromda.org/index.html

[Build AndroMDA] http://www.andromda.org/building.html

                 http://sourceforge.net/projects/andromda/files/?source=navbar

[mojo] https://stackoverflow.com/questions/9799392/trying-to-compile-mahout-but-getting-error-cannot-execute-mojo-resources-bui           

[maven - openjdk] https://stackoverflow.com/questions/9518523/installing-maven2-without-openjdk

[Parse-Dia-SQL] http://search.cpan.org/dist/Parse-Dia-SQL/

[left.subtree] http://left.subtree.org/2007/12/05/database-design-with-dia/


