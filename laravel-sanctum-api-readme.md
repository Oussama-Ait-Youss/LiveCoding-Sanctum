# 🛡️ Guide Live Coding : Authentification API avec Laravel Sanctum

Bienvenue dans ce guide d'implémentation de **Laravel Sanctum**.\
Ce document explique étape par étape comment créer un système
d'authentification par **Token** pour une **API REST Laravel**.

------------------------------------------------------------------------

# 🎯 Objectif de l'atelier

À la fin de ce guide vous aurez :

-   Une **API Laravel** capable d'enregistrer et connecter des
    utilisateurs
-   Un système de **génération de Tokens avec Sanctum**
-   Des **routes publiques** et **routes protégées**
-   La capacité de **tester votre API avec Postman ou Thunder Client**

------------------------------------------------------------------------

# 🛠️ Prérequis

Avant de commencer, assurez-vous d'avoir :

-   PHP **8.2+**
-   **Composer**
-   Une base de données (**MySQL ou SQLite**)
-   **Postman** ou **Thunder Client**

------------------------------------------------------------------------

# 🚀 Étape 1 : Initialisation du projet

Créer le projet Laravel.

``` bash
composer create-project laravel/laravel api-sanctum-demo
cd api-sanctum-demo
```

Configurer ensuite la base de données dans le fichier **.env**.

Exemple :

``` env
DB_DATABASE=api_sanctum
DB_USERNAME=root
DB_PASSWORD=
```

------------------------------------------------------------------------

# 🚀 Étape 2 : Installer le support API

Dans **Laravel 11+**, les API ne sont pas activées par défaut.

Exécuter :

``` bash
php artisan install:api
```

Cette commande :

-   crée `routes/api.php`
-   installe **Laravel Sanctum**
-   prépare les migrations

Ensuite lancer :

``` bash
php artisan migrate
```

------------------------------------------------------------------------

# 🧑‍💻 Étape 3 : Préparer le modèle User

Ouvrir :

    app/Models/User.php

Vérifier la présence de **HasApiTokens**.

``` php
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;
}
```

------------------------------------------------------------------------

# 🎮 Étape 4 : Création du AuthController

Créer le contrôleur :

``` bash
php artisan make:controller AuthController
```

Puis implémenter :

-   Register
-   Login
-   Logout

Fonctions principales :

### Register

-   validation des données
-   création de l'utilisateur
-   génération du token

### Login

-   vérification email/password
-   génération du token

### Logout

-   suppression du token courant

------------------------------------------------------------------------

# 🛣️ Étape 5 : Définition des routes

Fichier :

    routes/api.php

## Routes publiques

``` php
Route::post('/register', [AuthController::class, 'register']);
Route::post('/login', [AuthController::class, 'login']);
```

## Routes protégées

``` php
Route::middleware('auth:sanctum')->group(function () {

    Route::post('/logout', [AuthController::class, 'logout']);

    Route::get('/profile', function (Request $request) {
        return $request->user();
    });

});
```

------------------------------------------------------------------------

# ▶️ Lancer le serveur

``` bash
php artisan serve
```

API disponible sur :

    http://127.0.0.1:8000

------------------------------------------------------------------------

# 🧪 Test avec Postman

## 1️⃣ Register

POST

    /api/register

Body JSON :

``` json
{
  "name": "Student",
  "email": "student@test.com",
  "password": "password123"
}
```

Réponse :

-   user
-   token

------------------------------------------------------------------------

## 2️⃣ Accès sans Token

GET

    /api/profile

Résultat :

    401 Unauthorized

------------------------------------------------------------------------

## 3️⃣ Accès avec Token

Header :

    Authorization: Bearer TOKEN

Résultat :

Profil utilisateur.

------------------------------------------------------------------------

## 4️⃣ Logout

POST

    /api/logout

Le token est supprimé.

------------------------------------------------------------------------

# 🎓 Résumé

-   Les API Laravel sont **stateless**
-   **Sanctum** permet de générer des tokens
-   Les tokens sont stockés dans :

```{=html}
<!-- -->
```
    personal_access_tokens

-   Les routes protégées utilisent :

``` php
auth:sanctum
```

-   Le client doit envoyer :

```{=html}
<!-- -->
```
    Authorization: Bearer <token>

------------------------------------------------------------------------

# ✅ Conclusion

Vous avez maintenant :

-   une **API Laravel sécurisée**
-   une **authentification par token**
-   des **routes protégées**

Vous pouvez maintenant connecter :

-   un **Frontend React / Vue**
-   une **application mobile**
-   ou toute autre **application cliente**.
