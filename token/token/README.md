# Projet d'Authentification JWT avec Spring Boot

## Description
Ce projet est une implémentation d'authentification stateless utilisant JWT (JSON Web Tokens) avec Spring Boot. Il fournit une API RESTful sécurisée avec différents niveaux d'autorisation pour les utilisateurs et les administrateurs.

## Technologies Utilisées
- Java 8
- Spring Boot 2.7.18
- Spring Security
- JWT (JSON Web Tokens)
- H2 Database
- Maven
- Lombok

## Configuration Requise
- JDK 1.8 ou supérieur
- Maven 3.x

## Structure du Projet

### Configuration (`config/`)
- **SecurityConfig.java**
  - Configuration de Spring Security
  - Définition des règles d'autorisation
  - Configuration des filtres JWT
  - Gestion des beans de sécurité

### Contrôleurs (`controllers/`)
- **UserController.java**
  - Gestion des endpoints d'authentification
  - Points d'accès API pour les utilisateurs et administrateurs
  - Traitement des requêtes de connexion et d'inscription

### Entités (`entity/`)
- **AuthRequest.java**
  - Modèle pour les requêtes d'authentification
  - Contient email et mot de passe
- **UserInfo.java**
  - Entité principale utilisateur
  - Stockage des informations utilisateur en base de données

### Filtres (`filter/`)
- **JwtAuthFilter.java**
  - Intercepte les requêtes entrantes
  - Valide les tokens JWT
  - Gère l'authentification par token

### Repositories (`repository/`)
- **UserInfoRepository.java**
  - Interface JPA pour l'accès aux données utilisateur
  - Méthodes de recherche personnalisées

### Services (`service/`)
- **JwtService.java**
  - Génération et validation des tokens JWT
  - Gestion des claims JWT
- **UserInfoDetails.java**
  - Implémentation de UserDetails pour Spring Security
  - Conversion des entités utilisateur
- **UserInfoService.java**
  - Logique métier pour la gestion des utilisateurs
  - Implémentation de UserDetailsService

### Application Principale
- **TokenApplication.java**
  - Point d'entrée de l'application Spring Boot
  - Configuration principale

### Ressources
- **application.properties**
  - Configuration de la base de données H2
  - Paramètres de l'application
  - Configuration des logs

## Points d'Accès API

### Endpoints Publics
- `POST /auth/login` - Authentification et génération du token
- `POST /auth/addNewUser` - Création d'un nouvel utilisateur
- `GET /auth/welcome` - Message de bienvenue (non sécurisé)

### Endpoints Protégés
- `GET /auth/user/userProfile` - Accès utilisateur (ROLE_USER requis)
- `GET /auth/admin/adminProfile` - Accès administrateur (ROLE_ADMIN requis)

## Configuration de la Base de Données
La base de données H2 est configurée avec les paramètres suivants :
- URL: `jdbc:h2:mem:jwt`
- Console H2: `http://localhost:8080/h2-console`
- Utilisateur: `sa`
- Mot de passe: `` (vide)

## Exemple d'Utilisation

### 1. Création d'un Utilisateur
```bash
POST /auth/addNewUser
Content-Type: application/json

{
    "name": "Test User",
    "email": "user@example.com",
    "password": "password123",
    "roles": "ROLE_USER"
}
```

### 2. Authentification
```bash
POST /auth/login
Content-Type: application/json

{
    "email": "user@example.com",
    "password": "password123"
}
```

### 3. Utilisation du Token
```bash
GET /auth/user/userProfile
Authorization: Bearer <votre_token_jwt>
```

## Sécurité
- Authentification stateless avec JWT
- Encodage des mots de passe avec BCrypt
- Validation des tokens JWT
- Gestion des rôles (USER, ADMIN)

## Configuration Maven
Les principales dépendances incluent :
- spring-boot-starter-security
- spring-boot-starter-web
- spring-boot-starter-data-jpa
- jjwt-api (version 0.12.5)
- h2database
- lombok

## Notes de Développement
- La durée de validité du token JWT est de 30 minutes
- Les mots de passe sont encodés avant stockage
- La configuration CORS est désactivée pour le développement

