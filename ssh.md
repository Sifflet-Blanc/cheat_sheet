[< Home](README.md)
# Configuration de git et du ssh
- Création des bon fichier s'ils n'existent pas déjà
  ```
  mkdir ~/.ssh
  chmod 700 ~/.ssh
  touch ~/.ssh/known_hosts
  chmod 644 ~/.ssh/known_hosts
  ```
- Création de la clé ssh
  ```
  cd ~
  ssh-keygen -t rsa
  ```
- Activation de la cle <br/>
  Dans la suite `id_rsa` fera référence à la clé crée si vous l'avez nommé autrement remplacez donc tout les `id_rsa` par le nom de votre cle
  ```
  echo "eval `ssh-agent`" >> ~/.bashrc
  echo "ssh-add ~/.ssh/id_rsa" >> ~/.bashrc
  source ~/.bashrc
  ```
- Copier le contenu de `~/.ssh/id_rsa.pub` dans la section ssh du profil perso [github](https://github.com/settings/keys)/[gitlab](https://gitlab.com/-/user_settings/ssh_keys) dans l'option nouvelle cle
- Tester le bon fonctionnement de la cle ssh avec :
  ```
  ssh -T git@github.com
  ```
- Si installation :
  ```
  git clone git@github.com:Sifflet-Blanc/Manga_Reader.git
  ```
- Si le git et déjà cloné et que c'est une modification :
  - verifier que on pointe vien ver l'url ssh du repo avec : 
    ```
    git remote -v
    ```
    le resultat doit ressembler à `git@github.com:pseudo/repo.git` et pas à `https://github.com/pseudo/repo.git`
  - on changer l'url si necessaire avec la commande suivante :  
    ```
    git remote set-url origin git+ssh://git@github.com/username/reponame.git
    ```
  - Tester le bon fonctionnement du ssh avec :
    ```
    git remote show origin
    ```
