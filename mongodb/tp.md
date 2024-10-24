# MOVIES COLLECTION

## 1 - Créer un film
```
db.movies.insertOne(
    {
        "_id":"matrx-000-v2"
        "time":"1h40",
        "title":"Matrix 2",
        "year": 2000,
        "actor":"Keanu Reeves",
        "ratings":[
           {"user":"Baptiste",note:9},
           {"user":"Keryan",note:8},
           {"user":"Nassim",note:10}
        ],
        "genres":[
            "science-fiction","action"
        ]
    }
)