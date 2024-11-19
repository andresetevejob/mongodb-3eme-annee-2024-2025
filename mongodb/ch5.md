# MONGODB CRUD API

## Insertion
Avant MongoDB 3.2 la méthode insert permettait d'insérer un ou plusieurs documents, depuis ladite version une nouvelle sémantique a été mise en place afin de faciliter la compréhension des opérations

* InsertOne
```
db.personnes.insertOne({"nom": "Durand", "prenom": "Françoise"})
```

* InsertMany
```
db.personnes.insertMany([
{"nom": "Durand", "prenom": "Robert"},
{"nom": "Dupont", "prenom": "France"}
])
```

* Ordre des enregistrements
```
 db.personnes.insertMany([
  {"nom": "Durand", "prenom": "Robert"},
  {"nom": "Dupont", "prenom": "France"}
 ],{ordered:true})
```

* Spécifier un identifiant

Dans les enregistrements, nous avons pu observé identifiant automatiquement généré
par mongodb pour chacun de nos enregistrements, si nous voulons spécifier un id
métier à notre enregistrement, nous pouvons le faire en valorisant le champ _id, lors de
l’enregistrement
```
db.personnes.insertMany([{ "_id": "pers1", "nom": "durand",
"prenom": "robert" }, { "_id": "pers1", "nom": "dupont",
"prenom": "france" }, { "_id": "pers2", "nom": "dupont", "prenom": "eric" }],
{"ordered": false})
```


## Modification des documents

* updateOne
```
 db.personnes.updateOne(
  {"nom": "dupont"},{$set: {"nom": "Dupont"} }
 )
```

* UpdateOne with upsert
si le paramètre upsert est à true,  une modification sera opérée sinon dans le cas
contraire une insertion se fera, si aucun document n’est concerné par le filtre.
```
db.personnes.updateOne(
{"prenom": "Georges"},{$set: {"nom": "Dupont"} },{“upsert”:true}
)
```


#### Les operateurs
##### $set

Cet opérateur renseigne la valeur du champ dans le document.
  - si le champ n’existe, il sera créé et valorisé avec la valeur précisé,sinon la valeur sera
modifié
  - si le nom du champ contient un point pour un champ non existant, un document
embedded sera créé ou modifié
  - si plusieurs pairs de clé-valeur sont spécifiés, plusieurs champ seront crées ou modifiées

```
 db.personnes.updateOne({"_id": "pers1"},
   {$set: {"age": "14",”loisirs”:[“football”,”voyages”]} }
 ) 
//les champs age et loisirs seront crées
```

```
 db.personnes.updateOne({"_id": "pers1"},
  {$set: {"infos.secuId": “0133467A”} }
 )
//La collection embedded infos sera crée
```
```
 db.personnes.updateOne({"_id": "pers1"},
  {$set: {"loisirs.0": “tennis”} }
 ) //football sera modifié par tennis
```
##### $inc
Cet opérateur incrémente le champ concerné par la valeur spécifié. si la valeur spécifié
est négative, une décrémentation sera appliqué
IL ne peut être utilisé que sur les champs numériques, sinon une erreur sera indiqué
par mongodb lors de la modification
```
{ $inc: { <field1>: <amount1>, <field2>: <amount2>, ... } }
```
 
 ##### Liste exhaustive
 ```
  https://www.mongodb.com/docs/manual/reference/operator/update-field/
 ```
* updateMany

* $push

* Renommer un champ de document
L'opérateur rename, permet de modifier le nom d’un champ existant dans un
document. Le premier est le champ cible et le second le nouveau nom du champ
```
db.personnes.updateMany({}, {$rename: { "prenom": "prenoms"}}) //Modification d’un seul champ
db.personnes.updateMany({}, {$rename: { "prenoms": "prenom", "age": "ages"}}) // Modification
de plusieurs champs
```
Attention toutefois lorsque vous effectuez un renommage sur un champ: s’il existe
un champ portant le nouveau nom, les valeurs contenues dans celui-ci seront
purement et simplement écrasées par celles qui proviennent du nouveau champ !
```
//si nous renommons le champ prenoms par nom
db.personnes.updateMany({}, {$rename: { "prenoms": "nom"}})
// les valeurs contenues dans prenoms écraseront celle de nom
```
IL est possible de renommer un champ contenu dans un sous document
```
db.personnes.insert(
{"prenom": "dupont", "adresse": {"ville": "Apt", "cp": "84400"}}
)
db.personnes.updateMany({}, {$rename: { "adresse.cp":
"adresse.code_postal"}})
```
Si vous ne pointez correctement le sous document sur le nom du nouveau champ,
le champ sera déplacé au niveau supérieur
```
db.personnes.updateMany({}, {$rename: { "adresse.cp": "code_postal"}})
// le champ code_postal sera déplacé dans le document personne plutôt qu’il reste dans le
//document adresse.
```


## Lecture de document

La méthode find permet de récuperer les documents d'une collection, au moyen d'un filtre, si le filtre est vide alors tous les documents de ladite collection sont concernées.

PS: Un filtre est aussi un document 

```
 db.maCollection.find() ou db.maCollection.find({})
// Tous les documents de la collection maCollection seront retournées 
```
{} = répresente un document vide

Lorsqu'on spécifie un document avec une clé valeur alors une restriction s'opère sur la recherche de document
```
db.maCollection.find({"age":14})
// retourne tous les documents dont l'age est égal à 14
```
* AND
```
db.maCollection.find({"username":"koffi","age":14})
```

* Projection
```
db.movies.find({},{"title":1,"year":1})
```
* Conditionnals
```
db.movies.find({"year" : {"$gte" : 2000, "$lte" : 1999}})
```
* $in
```
db.raffle.find({"ticket_no" : {"$in" : [725, 542, 390]}})
```

* $nin
```
db.raffle.find({"ticket_no" : {"$nin" : [725, 542, 390]}})
```
* $or
```
db.raffle.find({"$or" : [{"ticket_no" : 725}, {"winner" : true}]})
```
* Faire une requête sur les documents dont les champs sont inexistants
```
db.movies.find({"year":null})
```
#### Requête sur les tableaux

```
//recherche les movies qui ont le genre romance
db.movies.find({"genres":"romance"})
```
* $all
```
// recupère les films de la categorie sciences-fiction et drame
db.movies.find({"genres":{$all:['sciences-fiction','drame']}})
```

{"genres":{$all:['drame']}} est équivalent à {"genres":"drame"}
```
db.movies.find({"genres":{$all:['drame']}})
```


#### Requête sur les objets imbriqués
```
db.people.find({"name" : {"first" : "Joe", "last" : "Schmoe"}})
db.employe_info.find({"adresse" : {"NomRue" : "Guillaume", "CodePostal" : "59000"}})
// recupère les documents, dont les documents imbriquées correspondent au document passé en paramètre
```
si un nouveau champ est rajouté dans le sous-document, aucun document ne correspondra, car cette méthode fait une comparaison exacte et aussi tiens compte de l'ordre : {"last" : "Schmoe","first" : "Joe"}.

Pour pallier à cela il faut utilisé la notation pointé
```
db.people.find({"name.first" : "Joe", "name.last" : "Schmoe"})
```
* $elemMatch :

soit la collection suivante 
```
[
  {
    _id: ObjectId('6719e5b8704e97c353fbd150'),
    content: "Les flammes de l'amour",
    comments: [
      { author: 'joe', score: 8, comment: 'nice' },
      { author: 'mary', score: 6, comment: 'terrible post' }
    ]
  },
  {
    _id: ObjectId('6719e5da704e97c353fbd151'),
    content: 'Les joies du code',
    comments: [
      { author: 'joe', score: 3, comment: 'nice' },
      { author: 'mary', score: 7, comment: 'terrible post' }
    ]
  }
]
```
On souhaite afficher tous les posts au mary a fait un commentaire avec une note >=6
```
db.blog.find({"comments.author" : "mary", "comments.score" : {"$gte" : 6}})
```
La requête ci-dessus va nous tous les posts dans lesquels on a la fois un commentaire supérieur à ou égal à 6 ou dont l'auteur est Mary

Pour avoir un résultat correct, il nous faut un $elemMatch
```
db.blog.find({"comments" : {"$elemMatch" : {"author" : "mary", "score" : {"$gte" : 6}}}})
```

* countDocuments

Permet de compter les documents contenus dans une collection. Cette méthode prend aussi
un paramètre et des options
```
db.collection.countDocuments( <query>, <options> )
```
source : https://www.mongodb.com/docs/manual/reference/method/db.collection.countDocuments/

* distinct
Permet de récuperer des valeurs distinctes
```
 db.collection.distinct(field, query, options)
```
source : https://www.mongodb.com/docs/manual/reference/method/db.collection.distinct/

* findAndModify

Modifie et retourne un seul document.Par défaut ne retourne pas le document incluant les
modifications effectuées par le update.Pour retourner celui incluant les modifications
effectuées, il faut rajouter l’option new.

```
db.collection.findAndModify({
query: < document >,
sort: < document >,
remove: < booléen >,
update: < document >,
new: < booléen >,
fields: < document >,
upsert: < booléen >
});
```

La commande ci dessous modifie les prénoms commençants par r
```
db.personnes.findAndModify({
"query": { "prenom": /^r/i },
"sort": { "prenom": -1 },
"update": { $inc: { "nbmodifs": 1 } },
"new": true
})
```
IL possible de supprimer un document avec le findAndModify,en rajoutant l’option
remove valorisé à true.Ce dernier n’est pas compatible avec l’option new
```
db.personnes.findAndModify({
"query": { "prenom": /^r/i },
"sort": { "prenom": -1 },
"remove": true
})
```
Nous pouvons retourner un sous ensemble des informations du document avec
un findAndModify, au moyen du champ fields.Dans l’exemple ci-dessous, le
document retourné incluera le champ nbmodifs et exclura le champ _id
```
db.personnes.findAndModify({
"query": { "prenom": /^r/i },
"sort": { "prenom": -1 },
"update": { $inc: { "nbmodifs": 1 } },
"fields": {"_id": 0, "nbmodifs": 1},
"new": true
})
```
Enfin la méthode findAndModify, met également à notre disposition, un champ
pour réaliser des upserts.si le filtre ne cible aucun document, un nouveau en sera
créée
```
db.personnes.findAndModify({
"query": { "prenom": /^r/i },
"sort": { "prenom": -1 },
"update": { $inc: { "nbmodifs": 1 } },
"fields": {"_id": 0, "nbmodifs": 1},
"new": true,
"upsert":true
})
```

## Suppression de document
```
db.personnes.drop()
db.personnes.deleteMany({})
db.personnes.remove({})
db.personnes.deleteOne(
{"prenom": "Sébastien"}
)
db.personnes.remove({"prenom": "Sébastien"}, true) //supprime un seul document
```

## Remplacer un document
IL est possible de modifier un document au sein d’une collection grâce à la méthode
replaceOne
```
db.collection.replaceOne(filtre, document)
```
Lorsque nous faisons un replace, il faut spécifier les champs propriétés du document à
remplacer sinon, nous les perdrons.
```
db.personnes.replaceOne(
{"prenom": "robert"},
{"prenom": "Jean-Sébastien"}
)
// Toutes les propriétés contenues dans le document ayant pour prenom robert seront perdues
// pour les conserver, il faudrait les spécifier dans le replace
db.personnes.replaceOne(
{"prenom": "robert"},
{"prenom": "Jean-Sébastien",”nom”:”plyia”}
)
```
