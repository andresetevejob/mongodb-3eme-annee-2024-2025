# REST ARCHITECTURE - HTTP (Partie 2)

## 7 - Réponses
### 7.1 Structure de la requête
```
{Version}{SP}{Code Status}{SP}{Phrase Status}

```

Version : identique à celle de la requête
SP :  Espace
Code Status:  Code de retour
Phrase Status: description du code

### 7.2 Les différents code de retour

#### Les informations
* 100 Continue
Ce code status est rarement utilisé et informe simplement que la partie de la requête qui a déjà été reçue est valide. Il n'est pas envoyé par défaut, mais seulement dans des cas précis.

* 101 Switching protocol
Ce code status permet de changer le protocole ou la version du protocole utilisé lors de la communication. Le nouveau protocole à utiliser est spécifié par l'en-tête Upgrade.

#### Les succès
* 200 OK
Tout est bon, c'est la réponse la plus souvent employée.

* 201 Created
Peut être utilisé par exemple dans le cadre d'une requête PUT pour indiquer que le document a bien été uploadé.

* 204 No Content
La requête s'est bien déroulée, mais le corps de la réponse est vide.

* 206 Partial Content
Ce code est généralement utilisé dans le cadre d'une récupération de téléchargement, ou de l'utilisation du cache. Seule une partie du document demandé est renvoyée.

#### Les redirections

* 300 Multiple Choice
Ce code est utilisé quand on peut trouver plusieurs versions de la ressource (différence de format ou de langue par exemple).

* 301 Moved Permanently
Quand une ressource est déplacée définitivement, c'est ce code qui permet d'indiquer le déplacement notamment aux moteurs de recherche. La requête ayant généré l'erreur doit alors être renvoyée pour correspondre avec la nouvelle ressource.

* 307 Temporary Redirect
Ce code permet d'indiquer une redirection temporaire.

#### Les Erreurs côté client

* 400 Bad Request
Erreur générique.

* 401 Unauthorized ou Authorization required
Le client n'est pas censé avoir accès à la requête au vu de son niveau actuel d'identification. Il doit s'identifier de manière correcte. S'il ne peut remplir les conditions d'identification, alors les requêtes suivantes aboutiront à une erreur 403.

* 403 Forbidden
La ressource est interdite au client.

* 404 Not Found
La fameuse erreur 404, elle indique que la ressource demandée n'a pu être trouvée.

* 405 Method Not Allowed
Le client n'est pas censé pouvoir envoyer ce type de requête, une authentification est certainement nécessaire.

#### Les erreurs côtés serveurs
* 500 Internal Server Error
Erreur Interne au serveur, il n'est pas en état de répondre actuellement.

* 501 Not Implemented
Certaines fonctionnalités requises par les en-têtes ou la méthode employés ne sont pas supportées par le serveur.

* 503 Service Unavaible
Utilisé par exemple quand le serveur est surchargé.

* 505 HTTP Version Not Supported
Le serveur ne supporte pas la version du protocole HTTP qui a été utilisée.

### 7.3 Les entêtes de réponse

* Content-type et Content-length : 
Ces deux en-têtes sont censées indiquer le type MIME et la taille en octets du corps de la réponse. De plus, l'en-tête Content-type peut indiquer le charset utilisé dans le corps de la réponse (dans le cadre d'un type texte). Il est alors indiqué par la mention "charset=" suivie du nom du charset. Il suit le type MIME, en est séparé par un point-virgule. S'ils ne sont pas indiqués, c'est le client qui est seul responsable de leur éventuelle valeur par défaut.

* Location : 
Cet en-tête permet d'indiquer une redirection. À sa réception, le client est généralement censé renvoyer une requête sur l'adresse indiquée. Ce comportement dépend du code status renvoyé avec la réponse.

* Set-Cookie :
Cet en-tête permet d'indiquer au client des cookies à stocker. Sa valeur peut prendre une forme assez complexe. La forme par défaut est celle de l'en-tête de requête Cookie. Les formes plus évoluées consistent à l'indication d'informations telles que : la date de péremption (Expires), le domaine d'application (Domain), le chemin d'application (Path), etc. Toutes ces informations sont indiquées à la suite du couple nom/valeur de la même manière (nom de l'information, égal, valeur) et séparées entre elles et de celui-ci par un point-virgule.

### 7.4 Exemple de réponse
```
accept-ranges:
bytes
access-control-allow-origin:*
age:61264
alt-svc:clear
cache-control:public,max-age=86400
content-length:15948
content-security-policy:default-src 'none'; img-src 'self'; script-src 'unsafe-inline'; style-src 'self'
content-type:application/json
date:Mon, 18 Nov 2024 11:08:40 GMT
etag:"4b89f7a0eccb74208727093906ac1f92"
```