Ressource mise au pluriels dans les requetes

Verbe HTTP
GET 
POST
PUT on envoie tout l'object (tout les champs) si en voulant modifier l'object on trouve qu'il existe pas on le creer 
PATCH 
DELETE

Code de retours :
100 - 199 information
200 - 299 Succès
300 - 399 Redirection
400 - 499 Erreur imputable au client
	401 Unauthorized = t'es pas authentifier
	403 Forbidden = ressource existe mais on peut pas y acceder 
500 - 599 Erreur serveur
