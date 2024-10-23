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

## Lecture de document

La méthode find permet de récuperer les documents d'une collection, au moyen d'un filtre, si le filtre est vide alors tous les documents de ladite collection sont concernées.

PS: Un filtre est aussi un document 

```
 db.maCollection.find() ou db.maCollection.find({})
// Tous les documents de la collection maCollection seront retournées 
```
{} = répresente une collection vide

Lorsqu'on spécifie un document avec une clé valeur alors une restriction s'opère sur la recherche de document
```
db.maCollection.find({"age":14})
// retourne tous les documents dont l'age est égal à 14
```
