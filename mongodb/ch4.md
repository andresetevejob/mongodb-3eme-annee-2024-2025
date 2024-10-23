# MongoDB Collection

## I - Collection
Convention de nommage des collections :
* Pas de chaîne vide “”
* Le nom de la collection ne doit pas contenir le caractère \0 (le caractère null)
* Ne doit pas être préfixé par system
*  Le nom de la collection ne doit pas contenir le caractère $

### 1 - Les collations
Elles permettent de définir les règles pour la comparaison des chaînes de
caractères,notamment en ce qui concerne l’accentuation des caractères ou leur casse
dans un langage donné.
Les collations peuvent être utilisées avec les collections, les vues ou encore les index

```
{
    locale: < chaîne de caractères >,
    caseLevel: < booléen >,
    caseFirst: < chaîne de caractères >,
    strength: < entier >,
    numericOrdering: < booléen >,
    alternate: < chaîne de caractères >,
    maxVariable: < chaîne de caractères >,
    backwards: < booléen >
}
```

* Locale : IL contient le langage utilisé, noté au format défini par l’ICU (International
Components for Unicode).Exple : fr (Français de France) et fr_CA(Français Canada)

* Strength : représente le niveau de comparaison qui sera effectué entre les chaînes de
caractères. Ce niveau est tel que défini par l’ICU : lorsqu’il vaut 1, le niveau de
comparaison est dit primaire, c’est-à-dire que la comparaison se fait uniquement en se
basant sur les caractères, sans tenir compte de leur accentuation éventuelle ou de la
casse (majuscule ou minuscule). Lorsque la valeur du champ strength est 2, le niveau de
comparaison est dit secondaire, c’est-à-dire qu’en plus de la comparaison primaire, les
accents sont désormais pris en compte. Le niveau de comparaison tertiaire est celui par
défaut et, en plus des critères du niveau secondaire, il prendra en compte la casse. Il
existe deux autres niveaux, mais les trois premiers restent les plus utilisés.

* caseLevel : sert lorsque le niveau de comparaison exigé dans strength est primaire ou secondaire. En
effet, ces deux niveaux sont les seuls qui ne prennent pas la casse en considération. Lorsque caseLevel
vaut true (ce qui n’est pas le cas par défaut), alors la casse sera prise en compte.

* caseFirst : sert uniquement si le niveau de comparaison est tertiaire et détermine l’ordre de tri :
lorsqu’il vaut upper les majuscules apparaissent avant les minuscules dans le tri, lorsqu’il vaut lower
l’inverse se produit. La dernière valeur, qui est celle par défaut, est off. À quelques subtilités près, elle
fait la même chose que lower.

* numericOrdering: indique si les chaînes de caractères contenant des nombres doivent être traitées
comme des chaînes de caractères (quand il vaut faux, ce qui est le cas par défaut) ou bien comme des
numériques (quand il vaut vrai).

* alternate : dit si les caractères de ponctuation ou les espaces doivent être pris en compte lors
de la comparaison de chaînes de caractères. Si tel est le cas, la valeur de ce champ sera
non-ignorable (la valeur par défaut), sinon elle vaudra shifted.
* maxVariable : n’a de raison d’être que lorsque le champ alternate vaut shifted : lorsque
maxVariable prend la valeur punct, les espaces et la ponctuation ne sont pas considérés
comme des caractères de base. Lors qu’il vaut space, cela vaut uniquement pour les espaces.
* backwards: détermine la direction de la comparaison : s’il vaut faux (comme c’est le cas par
défaut), alors la direction de la comparaison sera du début vers la fin, *normalization: <booléen>
précise si le texte nécessite une normalisation qui sera faite le cas échéant. Par défaut, aucune
normalisation n’est effectuée

## II - Gestion des collections
* Bien qu’on puisse créer automatiquement une collection en y insérant un document, il
est possible de créer une collection avec la méthode createCollection.
* L’utilisation de cette méthode est conseillé, si nous voulons appliquer certaines options
particulières sur notre collection, notamment: spécifier le nombre de document que
doit contenir la collection (capped collection), appliquer certaines validations, avant la
création des documents ou encore appliquer une collation

```
db.createCollection("macollection")
```
ou en utilisant une collation
```
db.createCollection("macollection",
{
    "collation": {
    "locale": "fr",
    "caseLevel": false,
    "strength": 3,
    "numericOrdering": false,
    "alternate": "non-ignorable",
    "backwards": false,
    "normalization": false
}
})
```

Suppression de collection
```
db.macollection.drop()
```

Renommer une collection
```
db.collectionarenommer.renameCollection("collectionrenommee")
```
Si le nom que nous voulons donner à notre collection est déjà porté une autre
collection, une erreur nous sera affichée par mongodb.
Alors il faut passer le paramètre true à la méthode renameCollection

```
db.collectionarenommer.renameCollection("collectionrenommee", true)
```
