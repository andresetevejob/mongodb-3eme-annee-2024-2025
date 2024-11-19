# REST ARCHITECTURE (Partie 3)

## II - REST ARCHITECTURE

### 1 - Definition

Rest une architecture logiciel basée sur le protocol HTTP.
Rest n'est pas : 
* Un protocol
* Un format
* Un Framework

IL a été crée par Roys Thomas Fielding

###  2 - Caractéristiques de l'architecture REST

* Stateless (sans état)
* Les ressources sont identifiés par une URI
* Les ressources sont accessibles au moyen des verbes HTTP (GET-POST-PUT-DELETE)
* Les ressources peuvent avoir plusieurs répresentations
* Les ressources possèdent un format

### 3 - Ressource
###### Quelque chose d'identifiable sur un système, ce peut être :

 - une ligne de base de données (relationnelle)
 - Un document (mongodb)
 - Le resultat d'un calcul
 - Un fichier (photo - videos, etc....)

### Le format de la ressource
 - Json
 - Xml
 - HTML
 - JPEG,PNG, GIF
 - MP3,MP4

Exemple: 
```
 - Format JSON
 {
    "id":1,
    "libelle":"Test"
 }
```
###### Identifiable par une URI
 - https://www.monapp/api/v1/books/1

```
 - books: ressource
 - 1 : idenitifiant de la ressource dans le système
```
### 4 - Les operations
Elles sont appliquées sur les ressources, elles sont basées sur les méthodes HTTP

* RETRIEVE : GET
* CREATE : POST
* UPDATE : PUT
* DELETE : DELETE

### 5 - REST Avancé

#### Cache-Control :
L'en-tête HTTP Cache-Control contient des directives (c'est-à-dire des instructions), dans les requêtes et dans les réponses, pour contrôler la mise en cache dans les navigateurs et caches partagées (par exemple les proxies, CDN).
Les directives pour le cache :

   * private : La donnée peut être stocker dans le cache du navigateur

   * public : La donnée peut être cachée au niveau du proxy ou du cdn (Content - Devlivery - Network)

   * no-cache : Cette directive signifie que les versions mises en cache de la ressource demandée ne peuvent pas être utilisées sans vérifier au préalable s'il existe une version mise à jour. Cela se fait généralement à l'aide d'un ETag.

   * no-store: Une réponse avec une directive « no-store » ne peut jamais être mise en cache, à quelque endroit que ce soit. Cela signifie que chaque fois qu'un utilisateur demande ces données, une requête doit être envoyée au serveur d'origine pour obtenir une nouvelle copie. Cette directive est généralement réservée aux ressources qui contiennent des données extrêmement sensibles, telles que les informations de compte bancaire.

   * max-age : indique que la réponse reste fraîche jusqu'a N secondes après la génération de cette dernière par le serveur


#### Etag

 - Eviter des collisions en vols : A l'aide des    en-têtes ETag et If-Match (en-US), vous pouvez  détecter les collisions d'édition en vol.
  Par exemple, lors de l'édition de MDN, le contenu actuel du wiki est haché et placé dans un Etag dans la réponse :
```
  ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"

```
 Lors de la sauvegarde des modifications d'une page wiki ("post" des données), la requête POST contiendra l'en-tête If-Match (en-US) contenant les valeurs ETag par rapport auxquelles vérifier la péremption.

```
If-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"

```
Si les hachages ne correspondent pas, cela signifie que le document a été modifié entre-temps, et une erreur 412 Precondition Failed est déclenchée.

https://fideloper.com/etags-and-optimistic-concurrency-control

 - Mise en cache des ressources inchangées : Un autre cas d'utilisation typique de l'en-tête ETag est de mettre en cache les ressources qui sont inchangées. Si un utilisateur visite à nouveau une URL donnée (qui a un ensemble d'ETag), et qu'elle est périmée, c'est à dire, trop ancienne pour être considérée comme utilisable, le client enverra en même temps la valeur de son ETag dans un champ d'en-tête If-None-Match :
 ```
   If-None-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```
 Le serveur comparera l'ETag du client (envoyé avec If-None-Match) à l'ETag de sa version en cours de la ressource, et si les deux valeurs correspondent (c'est-à-dire que la ressource n'a pas changé), le serveur renverra un statut 304 Not Modified, sans aucun corps, qui indiquera au client que sa version mise en cache de la réponse est toujours bonne à utiliser (actuelle).




#### RateLimiter
 - https://learn.microsoft.com/fr-fr/dotnet/core/extensions/http-ratelimiter
 - https://systemsdesign.cloud/SystemDesign/RateLimiter
 - https://www.cloudflare.com/fr-fr/learning/bots/what-is-rate-limiting/
 - https://redis.com/glossary/rate-limiting/


#### Pagination

 - Offset Pagination
 - KeySet Pagination
 - 

 https://vladmihalcea.com/sql-seek-keyset-pagination/
 https://dev.to/pragativerma18/unlocking-the-power-of-api-pagination-best-practices-and-strategies-4b49

#### Streaming Response

 - https://gist.github.com/CMCDragonkai/6bfade6431e9ffb7fe88
 - https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding

### 6 - Sécurité

* Basic Authentication :

 - https://swagger.io/docs/specification/authentication/basic-authentication/

* Api Keys :


* Authentication Bearer :

 - 

* Oauth 2 :

  - https://zestedesavoir.com/articles/1616/comprendre-oauth-2-0-par-lexemple/


* OpenID Connect :

### 7 - Bonnes pratiques pour designer une API REST

- https://blog.octo.com/designer-une-api-rest


### 8 - Ressources additionnelles
- https://technicalsand.com/streaming-data-spring-boot-restful-web-service/
- https://datatracker.ietf.org/doc/html/rfc2616#section-13.4
- https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/Cache-Control
- https://www.cloudflare.com/fr-fr/learning/cdn/glossary/what-is-cache-control/
- https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/ETag#mise_en_cache_des_ressources_inchang%C3%A9es
- https://dzone.com/articles/concurrency-control-in-rest-api-with-spring-framew