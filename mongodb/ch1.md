# MONGODB - PERSISTENCE

## I - Persistance
La persistance est un concept en informatique qui permet :
* Stockage des données sur un disque via un outil (Base de données , Fichiers)
* Récupération des données au prochain redémarrage de l’application

## II - Bases de données rélationnelles
* Elles ont longtemps été la technologie de stockage par défaut dans le domaine des
développements des applications informatiques
* Souvent les entreprises sont liées à un fournisseur de Base de données
* Elles s’appuient le modèle relationnel
(https://openclassrooms.com/fr/courses/4449026-initiez-vous-a-lalgebre-relationne
lle-avec-le-langage-sql/4449033-decouvrez-le-concept-de-relation)
* Utilise le langage SQL pour l'interrogation des données

## III - Avantages des Bases relationnelles
* Gestion de la concurrence
* Intégration des données (Base de données partagées entre plusieurs applications)
* Modèle standard (Le dialect SQL diffère peu entre les fournisseurs de SGBD)

## IV - Inconvénients des Bases relationnelles
* N’ont pas été conçues à la base pour fonctionner en cluster
* IL y’a un décalage de correspondance entre les données venant de la base et les
structures de données utilisées dans l’application

## V - Histoire du Nosql
https://fr.linkedin.com/pulse/le-nosql-cest-du-sql-rudi-bruchez


## VI - Mongodb vs Bases de données relationnelles


| Fonctionnalités             | MongoDB          | Bases de données rélationnelles |
| :---------------------------|:---------------:| -----:|
| Tuple                       |   Non            |  Oui |
| Document                    |   Oui             | Non
| Champ valorisé à null  | Absent          |    Contiendra la valeur Null |
| Schéma uniforme pour tous les enregistrements  | Non          |    Oui |
| Ordre des champs d'un enregistrement peut être différents des autres  | Oui          |    Non |

## VII - Correspondance entre MongoDB et une base relationnelle
![composant-1](images/correspondance.png "Le titre de mon image")