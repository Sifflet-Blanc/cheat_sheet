[Accueil](README.md)
# Git

## Config
`git config --global user.email some@thing.com`

`git config --global user.name someotherthing`

`git config --global alias.someAlias "Command"`

`git config --global core.editor "vim"`

My config
```bash
git config --global alias.tree "log \
--graph --abbrev-commit --decorate \
--format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' \
--all"
git config --global alias.cane "commit --amend --no-edit"
git config --global merge.ff only
git config --global pull.rebase true
git config --global core.editor "vim"
```

## Initialisation
Du debut :
`git init`
Deja existant 
`git clone someUrl`

Fichier pour ignorer des fichiers:
`.gitignore`

## Branch
Information sur les branches locales
`git branch`

Change HEAD de branche 
`git checkout <branch name>`
_Pour creer une branche rajouter -b_

ou
`git switch <branch name>`
    _Pour creer une branche rajouter -c (ici ne change pas de branche a la création)_

Detruire une branche local
`git branch -d <branch name>`

Branche distante 
`git push origin --delete <branch name>`

Pour remonder sur un commit anterieur d'une branche
`git checkout <branch name>~<-nth commit>`


## Merge 
`git merge <branch name>` _en etant sur la branche dans la quelle on veux merge_
- `--no-ff` force la creation d'un commit de merge

Mieux ! 
`git rebase <nom de la branche sur la quelle on veux merge/commit>` _Met les commit de la branche actuelle au dessus de la branche de rebase_
- _la branche de rebase reste donc derriere_
- -i rend le rebase interactif (prendre un commit avant généralement pour avoir un comit a jouer de base)
    - pick ne fait rien
    - reword change le texte associé au commit 
    - squash permet de fusione le commit avec le precedent 
    - fixup prend automatiquement le message du dernier commit
    - drop detruit le commit ! ou supprimer la ligne
    - edit changer le commit 
        - git add
        - git commit --amend 
        - git rebase --continue
    - on peux changer l'ordres des lignes et cela changera l'ordre des commit joué

necessite un 
`git merge --ff-only` _--ff-only permet de ne pas creer de commit_

- en cas de conflit (potenciellement 1 par commit)
    - changer les fichiers dans les quels il y a des conflits
    - puis git add 
    - puis `git rebase --continue`

- En cas de probleme on peux tout annuler avec 
    `git rebase --abort` 

- Pour le repo distant apres il faut 
    `git push` _normalement echoue_
    `git push -f`

in the end before push we can check if everything is clean with 
`git clean -n`
`git clean -f`

## Ajout retrait de modif 
`git add -A` _-A ajoute tout alors que le . ajoute que le dossier courant (et les sous dossiers)_
`git reset .` _Contraire de add_

## Commit 
`git commit -m "Some message"` _creer un commit avec les changements locaux et un message_
    - --amend _integre les changements au dernier commit_

`git reset --soft <commit>` _revenir sur un commit precedent en gardant les actuels_
    - --hard _supprime les commits actuels_

## Stash
`git stash` _mettre de coter le travail_
`git stash pop` _recuperer le dernier stash_
`git stash drop` _detruit le dernier stash_


## Infos 
Voir la difference entre deux commit (par defaut l'actuel et le precedent)
`git diff <commit1> <commit2>` _difference entre le commit actuel et le dernier_

Infos en tout genre 
`git status`

Infos sur les commits 
`git tree`

Historique des actions locales (utile en cas de foirage)
`git reflog`

Ajouter un tag a un commit:
```bash
git tag <tag_name>
```
_les tags sont unique sur tout le projet_
    _on peut checkout sur un tag_

Pour push un tag on peut 
```bash
git push origin tag <tag_name>
```
ou moins bien 
```bash
git push --tags
```

On peut supprimer un tag avec 
```bash
git tag -d <tag_name>
```

Revert 
`git revert <commit>` _fait un commit qui fait l'action inverse du commit referencer, fait un commit ! (contrairement au reset)_ 

Ctrl-v -c 
`git cherry-pick <commit>` _copie un commit sur la branche actuelle_


## Remote 

Infos 
`git remote -v` _infos sur le repo distant_
`git remote add origin someGitRemoteUrl` associe le repertoire a un remotei

`git clone someGitRemoteUrl` _pour copier un repertoire distant_

`git fetch` _recupere les changements_
- -p --tags _recupere tout les changements_

`git pull`
- --rebase mieux (voir les infos sur rebase)
- --no-rebase (voir info sur no rebase)

`git push origin main` _push la branche locale sur la remote_
    _-u ou --set-upstream lie la branche locale a la remote (sert aussi pour push une nouvelle branche)_
    _une fois liée un simple `git push` suffit_
