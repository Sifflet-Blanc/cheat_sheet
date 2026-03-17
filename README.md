# cheat_sheet
some usefull command

[Docker](docker.md)

[FFMPEG](ffmpeg.md)

[Shell](shell.md)

[SSH](ssh.md)

python

- uninstall all pip package : 
  ```
  pip uninstall $(pip list --format=freeze --user | cut -d= -f1) -y
  ```
