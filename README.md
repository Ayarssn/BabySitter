# Babysitter Services – README

## 🧸 Présentation du projet
Babysitter Services est une application web permettant aux **clients** de rechercher, filtrer et réserver un(e) babysitter, tout en offrant aux **babysitters** un espace pour proposer leurs services. Un **agent** gère les retours, la communication et l'administration du système.

Le projet est développé dans le cadre du module **Développement Web Avancé**, en utilisant :
- **Laravel** (backend sans API REST)
- **Blade** pour l'affichage
- **React** pour certains composants dynamiques
- **AJAX** pour les requêtes asynchrones
- **MySQL** pour la base de données

---

## 🎯 Objectifs du projet
- Permettre à un client de :
  - créer un compte, se connecter
  - rechercher un babysitter par ville, disponibilité, tarif
  - consulter les profils babysitters
  - réserver un créneau
  - laisser un avis

- Permettre à un babysitter de :
  - créer un profil
  - définir ses horaires, son tarif et sa localisation
  - gérer ses réservations

- Permettre à un agent/admin de :
  - consulter toutes les réservations
  - gérer les utilisateurs (blocage, suppression)
  - consulter les feedbacks

---

## 🏗️ Architecture du projet
Le projet est basé sur Laravel, organisé selon une logique MVC légère.

### Structure principale
```
/babysitter-app
│
├── app/
│   ├── Http/Controllers
│   ├── Models (minimal ou vide si SQL manuel)
│   ├── Services
│   └── Helpers
├── resources/
│   ├── views/ (Blade)
│   └── js/ (React + scripts AJAX)
├── public/
│   ├── css/
│   ├── js/
│   └── uploads/
├── routes/web.php
├── database/migrations
├── config/
└── .env
```

### Fonctionnement général
- **Les pages** sont rendues via Blade.
- **Les interactions dynamiques** (recherche babysitter, filtres, réservation en AJAX) se font via :
  - `fetch()` ou `$.ajax()`
  - réponses JSON renvoyées par des routes Laravel classiques
- **React est utilisé uniquement comme composant isolé**, par exemple :
  - carte babysitter
  - calendrier
  - modale de réservation

---

## ⚙️ Fonctionnalités principales

### 🔍 Recherche babysitter
- Recherche par ville, localisation,service ...
- Filtrage par tarif, expérience, disponibilité ...
- Affichage dynamique via React

### 📝 Réservation
- Formulaire de réservation (Blade)
- Envoi via AJAX
- Vérification de disponibilité côté serveur

### ⭐ Feedback
- Les clients peuvent laisser une note et un commentaire
- L’agent peut consulter et supprimer les abus

### 👤 Gestion utilisateur
- Inscription / Connexion
- Espace client
- Espace babysitter

---

## 💻 Technologie & outils
| Technologie | Rôle |
|-------------|------|
| **Laravel** | Backend, logique métier |
| **Blade** | Affichage des pages |
| **React** | Composants dynamiques |
| **AJAX** | Requêtes asynchrones |
| **MySQL** | Base de données |
| **Vite** | Build JS/CSS |

---

## 🚀 Installation
### 1. Cloner le projet
```
git clone https://github.com/votre-repo/babysitter-services.git
cd babysitter-services
```

### 2. Installer les dépendances
```
composer install
npm install
```

### 3. Configurer l'environnement
Créer un fichier `.env` :
```
cp .env.example .env
```
Configurer la base de données dans `.env`.

### 4. Générer la clé de l'application
```
php artisan key:generate
```

### 5. Lancer le serveur
```
php artisan serve
npm run dev
```

---

## 📦 Exécuter le projet
Ensuite, ouvrez :
```
http://localhost:8000
```

---

## 📘 Documentation
Le projet inclut :
- une **convention de codage complète** (Convention_Codage.md)
- un README détaillé
- une architecture documentée

---

## 📄 Licence
Projet académique – Module Développement Web Avancé.
Non destiné à une utilisation commerciale.

---

## 📝 Note
Ce projet est en cours de développement. Certaines fonctionnalités peuvent évoluer selon les besoins pédagogiques.
