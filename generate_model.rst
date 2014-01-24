Generation du "model.py":
=========================
Génération du SQL avec "tedia2sql":
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

Generation du SQL avec "umbrello":
----------------------------------
Un autre essai de génération de sql a été fait avec [umbrello]. 
Il ne crée que les tables sans les attributs !!!
Ce problème existait pour les versions de [2008], 
mais notre version actuelle est bien supérieure (Version 2.8.5, KDE Version 4.8.5).
Un essai avec "tedia2sql" crée bien le fichier "schema.sql" à partir de ".xmi". 
Mais le résultat ne contient ni tables ni attributs!
Apparemment le format .xmi poserait pas mal de problèmes en fonction des versions d'UML et des logiciels [interopératibilité].
D'après cette précédente page, le meilleur générateur serait [AndroMDA].

Génération du SQL avec "AndroMDA":
--------------------------------
C'est un projet en licence BSD utilisable pour tout OS. 
Pour l'installer il faut aussi avoir maven2 [Build AndroMDA] 
    
    ~/andromda-bin-3.3 $ mvn install -Pandromda-full,local

Attention, il y a sur leur explication une commande à passer, mais elle ne passe pas das la console:

    ~/andromda-bin-3.3 $ MAVEN_OPTS=-XX:MaxPermSize=256m -Xmx512m
    
Problème de Java:
-----------------

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

References:
===========

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



