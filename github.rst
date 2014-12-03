the difference between git pull origin master and git pull origin/master:
    http://stackoverflow.com/questions/2883840/differences-between-git-pull-origin-master-git-pull-origin-master

Il faudra d'après leur conseils faire le premier, pour merger automatiquement les modifs du master distant.

mail des commits
----------------
Pouvoir réécrire les adresses mails erronées si le git config a pas été bien fait:
http://git-scm.com/book/en/Git-Tools-Rewriting-History#Reordering-Commits

Cloner +sieurs branches
-----------------------
Cloner une ou toutes les branches d'un dépot distant: 
https://stackoverflow.com/questions/67699/clone-all-remote-branches-with-git

$ git branch -a
$ git checkout -b experimental origin/experimental
$ git checkout -b <branch> --track <remote>/<branch>

Copier un dépot:
----------------
Même lien, réponse Nov 26. 
Attention dans les commentaires ils précisent qu'il faut modifier la config de git.
https://stackoverflow.com/questions/67699/clone-all-remote-branches-with-git

mkdir repo
cd repo
git clone --bare path/to/repo.git .git
git config unset core.bare
git reset --hard

