# REST ARCHITECTURE - HTTP (Partie 1)

## 1 - Définition

- HyperText Transfer Protocol
- Protocol d'échange d'informations
- Basé sur TCP/IP

IL s'agit d'un protocol sur échange unique initié par le client.IL est synchrone.

![alt text for screen readers](/images/http-schema.png)

## 2 - Structure
structure des requêtes et réponse
```
{Ligne d'introduction}{SEP}
{En-têtes séparés par des {SEP} }
{SEP}{SEP}
{Corps}
```
La ligne d'introduction est le seul element qui permet de différentier la requête de la réponse


##  3 - Requête HTTP

### 3.1 - La ligne d'introduction

```
{Méthode}{SP}{Page}{SP}{Version}
```
SP => Correspond au séparateur

#### 3.1.a - Méthode (source https://developer.mozilla.org/fr/docs/Web/HTTP/Methods)
- GET : La méthode la plus courante. Elle utiliser pour télécharger une ressource.Deux requêtes GET portant sur le même document doivent retourner doivent 

- HEAD :  La méthode HEAD demande une réponse identique à une requête GET pour laquelle on aura omis le corps de la réponse (on a uniquement l'en-tête).

- POST : La méthode POST est utilisée pour envoyer une entité vers la ressource indiquée. Cela entraîne généralement un changement d'état ou des effets de bord sur le serveur.

- PUT : La méthode PUT remplace toutes les représentations actuelles de la ressource visée par le contenu de la requête.

- DELETE : La méthode DELETE supprime la ressource indiquée.

- OPTIONS : La méthode OPTIONS est utilisée pour décrire les options de communications avec la ressource visée.

- TRACE : La méthode TRACE réalise un message de test aller/retour en suivant le chemin de la ressource visée.

- CONNECT : La méthode CONNECT établit un tunnel vers le serveur identifié par la ressource cible.




### 3.2 - La Page
désigne le chemin d'accès de la ressource qu'on veut consommer.
Deux éléments peuvent être ajoutés à ce chemin

- Les paramètres d'URL (paramètres GET)
- L'identificateur de fragment (fragment identifier)
```
  {NOM} {EQ} {Valeur}
```

### 3.3  - La version
 - HTTP/1.1 pour 1.1
 - HTTP/1.0 pout 1.0

Exemple : /api/v1/product?id=5 HTTP/1.1
chemin de la ressource : /api/v1/product
NOM : id
EQ: =
valeur: 5


##  4 - Les entêtes de requête
 - Host
Cet en-tête est le seul obligatoire pour les requêtes HTTP/1.1, c'est lui qui permet d'héberger plusieurs sites Web sur un même serveur.
Sa valeur est le domaine du site Web. Il est généralement spécifié juste après la Ligne d'Introduction.

- User-Agent
Cet en-tête permet d'indiquer la signature du programme effectuant la requête. C'est une chaîne de caractères qui permet d'identifier le programme. En général, il s'agit du nom complet du programme et de sa version.

- Content-type et Content-length
Ces deux en-têtes ne peuvent être spécifiés que dans le cadre d'une requête POST ou PUT. Ils indiquent respectivement le type MIME et la taille en octets du corps de la requête. S'ils ne sont pas spécifiés, c'est le serveur qui est seul responsable de leur éventuelle valeur par défaut.

- Cookie
Cet en-tête permet au client de fournir un cookie au serveur.
Sa valeur est simplement le nom et la valeur du cookie, séparés par un égal.

- D'autres en-têtes
De nombreux autres en-têtes ont été prévus dans le protocole HTTP. Ils servent par exemple à préciser la gestion du cache, la langue ou le format préféré pour la réponse, l'authentification du client, etc. Les Requests For Comments (RFC) du protocole HTTP vous fourniront tous les en-têtes [pour ses versions 1.0 et 1.1]. Tout en-tête non reconnu par le serveur doit théoriquement être ignoré.

## 5 - Le corps de la requête

IL peut être vide pour certaines méthodes : GET, HEAD, DELETE, CONNECT, TRACE et OPTIONS

## 6 - Exemple de requête HTTP
```
 POST

 POST /api/auth/signup HTTP/1.1
 Host: localhost:8080
 Content-Type: application/json
 Content-Length: 113

 {
    "username":"andre",
    "password":"toto",
    "email":"kouameandre43@gmail.com",
    "role":["customer"]
 }
 --------------------------------
 GET /api/categories/65521d25bd280c76705d913e HTTP/1.1
 Host: localhost:3000 
```

#### Ressources

 - http://miageprojet2.unice.fr/@api/deki/files/2369/=webservice_REST.pdf
 - https://mathieu-lemoine.developpez.com/tutoriels/web/http/?page=page-3#LIII
