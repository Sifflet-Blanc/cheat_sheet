# Install
Uninstall old package
```
sudo apt remove docker docker-engine docker.io containerd runc
```
Installer les dépendances requises
```
sudo apt update
sudo apt install ca-certificates curl gnupg
```
Ajouter la clé GPG officielle de Docker
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
Ajouter le dépôt officiel de Docker
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Installer Docker Engine
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Vérifier l'installation
```
sudo docker run hello-world
```
Ajouter votre utilisateur au groupe docker (pour eviter d'utiliser `sudo` a chaque fois)
```
sudo usermod -aG docker $USER
newgrp docker
```
Activer le démarrage automatique de Docker
```
sudo systemctl enable docker
```
# In cli 
```
docker stop $(docker ps -aq)
docker system prune -a --volumes
```
