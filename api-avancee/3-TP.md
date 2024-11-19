# Réaliser un service REST avec Spring Boot

## 1 - Prerequis

 - Installez intellij idea
 - Jdk 20
 - Mongodb 6
 - Créer le projet depuis spring initialzr


## 2 - Enoncé

IL s'agit de concevoir une API rest qui permet à utilisateur de sauvegarder des images sur la plateforme.
 - L'utilisateur doit pouvoir :
   - se créer un compte
   - s'authentifier 
   - pouvoir enregistrer ses fichiers
   - Voir la liste des fichiers
   - Supprimer ses fichiers

 ## 3 - Identification des ressources de la plateformes

  - User
  - Files
 <table>
   <tr>
      <td>Ressource</td>
      <td>URI</td>
   </tr>
   <tbody>
        <tr>
           <td>User</td>
           <td>/GET</td>
        </tr>
   </tbody>
 </table>