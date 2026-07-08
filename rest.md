**[< Home](README.md)**
# [REST](acronyme.md#rest)
## Ressource mise au pluriels dans les requetes

## Verb HTTP
- `GET` send back data
- `POST` create a data
- `PUT` we send all the field of the data we want to update and if it's not here create it  
- `PATCH` update a specific part of a data, if it's not here raise an error
- `DELETE` delete a data 

## Code de retours
- 100 - 199 info
- 200 - 299 Succes
- 300 - 399 Redirection
- 400 - 499 Client error
	- 401 Unauthorized = t'es pas authentifier
	- 403 Forbidden = ressource existe mais on peut pas y acceder 
- 500 - 599 Erreur serveur
