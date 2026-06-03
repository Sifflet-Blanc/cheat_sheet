[Accueil](README.md)
# Python 

uninstall all pip package : 
  ```
  pip uninstall $(pip list --format=freeze --user | cut -d= -f1) -y
  ```