# Convention de Codage – Projet Babysitter Services

##  Objectif
Ce document définit les standards de codage, l'architecture, les conventions de nommage et les bonnes pratiques du projet **Babysitter Services**, développé avec :
- **Laravel (sans API REST)**
- **React utilisé pour des composants intégrés dans Blade**
- **AJAX (fetch / jQuery) pour les interactions asynchrones**
- **MySQL**

 **Important : ce projet n'utilise pas d’API REST, ni d’ORM avancé type Eloquent.**
- Les contrôleurs Laravel renvoient directement des vues Blade.
- Les interactions dynamiques utilisent AJAX pour communiquer avec les routes Laravel.
- Les modèles Laravel peuvent être utilisés uniquement pour la connexion DB basique (ou requêtes SQL directement via DB::select()).
- Le front React est utilisé comme composants isolés, pas comme Single Page Application.

---

## Structure du projet

### **1. Structure Laravel**
```
/app
    /Models
    /Http
        /Controllers
        /Requests
    /Providers
/bootstrap
/config
/database
    /migrations
    /seeders
/public
/resources
    /css
    /views
    /js           # Code React
/routes
    web.php
```

### **2. Structure React**
```
/resources/js
    /components
    /pages
    /services
    /context
    /hooks
```

---

##  Architecture du projet

### Backend (Laravel sans API / sans ORM)
- **Controllers** : gèrent les pages et les actions utilisateur.
- **Vues Blade** : HTML + composants React intégrés si besoin.
- **AJAX** : communication asynchrone via routes classiques (`POST /update-reservation`).
- **Base de données** : MySQL + requêtes SQL classiques.
- **Aucun ORM obligatoire** : on peut utiliser :
  - `DB::select()`
  - `DB::insert()`
  - `DB::update()`
  - `DB::delete()`

###  Frontend (React en composants dans Blade)
- React n'est pas utilisé pour gérer toute l'application.
- Il est utilisé pour :
  - des listes dynamiques (ex : liste babysitters)
  - un calendrier interactif
  - un formulaire intelligent
- Les données sont chargées via AJAX et insérées dans React.

### Communication Laravel ↔ React
- Aucun contrôleur d’API.
- AJAX envoie une requête à Laravel :
```
POST /fetch-babysitters
```
- Laravel renvoie un JSON simple.
- React met à jour l’interface.

---

## Conventions de nommage (Adaptées au projet sans API)

### Backend (Laravel sans API)
- **Contrôleurs** : `PascalCase` + suffixe `Controller`
  - `BabysitterController`
  - `ClientController`
  - `ReservationController`
- **Méthodes** : camelCase
  - `searchBabysitters()`
  - `saveReservation()`
- **Routes Laravel**
  - snake_case pour les URLs : `/create_reservation`
  - Nom des routes : snake_case : `->name('create_reservation')`
- **Views Blade** : kebab-case
  - `babysitter-list.blade.php`
  - `client-dashboard.blade.php`

### Frontend (React intégré)
- Fichiers React : kebab-case
  - `babysitter-card.jsx`
  - `modal-feedback.jsx`
- Composants React : PascalCase
  - `BabysitterCard`
- Hooks : camelCase
  - `useModal()`

---

### **Backend (Laravel)**
- **Classes** : PascalCase → `BabysitterController`
- **Méthodes & variables** : camelCase → `createBooking()`
- **Tables MySQL** : snake_case pluriel → `babysitters`, `reservations`
- **Colonnes** : snake_case → `first_name`, `availability_days`
- **Routes API** : kebab-case → `/api/babysitters-by-city`

### **Frontend (React)**
- **Composants/pages** : PascalCase → `BabysitterCard.jsx`
- **Fichiers** : kebab-case → `booking-page.jsx`
- **Hooks** : `useSomething()`
- **Services API** : camelCase + suffixe Service → `bookingService.js`

---

##  Bonnes pratiques Laravel (sans ORM)

### 1. Requêtes SQL avec Query Builder ou DB::statement
```
$results = DB::select('SELECT * FROM babysitters WHERE city = ?', [$city]);
```

### 2. Pas d’Eloquent obligatoire
Si utilisation minimale du modèle :
```
class Babysitter {
    public static function all() {
        return DB::select('SELECT * FROM babysitters');
    }
}
```

### 3. Validation dans les contrôleurs ou `FormRequest`
```
$request->validate([
    'email' => 'required|email',
]);
```

### 4. Communication AJAX sans API
Exemple :
```
$.post('/get_babysitters', { city: 'Rabat' })
 .done(function(data) {
    console.log(data);
 });
```

---

### **1. Eloquent**
```
public function babysitter(): BelongsTo {}
public function reservations(): HasMany {}
```

### **2. Validation via Form Requests**
```
public function rules(): array {
    return [
        'email' => 'required|email',
        'date' => 'required|date|after:today',
    ];
}
```

### **3. Réponses API standardisées**
```
return response()->json([
    'status' => 'success',
    'data' => $babysitters
]);
```

---

## Bonnes pratiques React (composants locaux)

### Utilisé avec Blade, pas en SPA
```
<div id="babysitter-list"></div>
<script src="/js/components/babysitter-list.jsx"></script>
```

### Appels AJAX depuis React
```
useEffect(() => {
  fetch('/get_babysitters')
    .then(res => res.json())
    .then(data => setList(data));
}, []);
```

### Rendu React enclenché par Blade
```
@vite(['resources/js/app.jsx'])
```

---

### **Composants fonctionnels**
```
export default function BabysitterCard({ data }) {
  return (
    <div className="card">
      <h3>{data.name}</h3>
    </div>
  );
}
```

### **Appels API via axios**
```
import axios from "axios";

export const getBabysitters = async () => {
    return await axios.get("/api/babysitters");
};
```

---

## Convention AJAX (sans API REST)

### Les routes Laravel retournent du JSON
```
return response()->json([
  'status' => 'ok',
  'babysitters' => $list
]);
```

### Exemple d’appel depuis React
```
fetch('/search_babysitters', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ city: 'Casablanca' })
})
.then(res => res.json())
.then(data => setResults(data.babysitters));
```

---

### Réponse standard
```
{
  "status": "success",
  "data": {},
  "message": null
}
```

### Erreur
```
{
  "status": "error",
  "message": "Email already exists"
}
```

---

## Base de données (MySQL)

### Tables
- `babysitters`
- `clients`
- `reservations`
- `availability`

### Colonnes
- snake_case obligatoire
- FK : `{entité}_id`

### Exemples
```
CREATE TABLE babysitters (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(255),
  city VARCHAR(255),
  hourly_rate FLOAT,
  experience_years INT,
  rating FLOAT,
  description TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

---

- **Tables** : snake_case pluriel → `clients`, `babysitters`, `reservations`
- **PK** : `id`
- **FK** : `{singulier}_id` → `client_id`
- **Timestamps Laravel** : `created_at`, `updated_at`

---

##  Sécurité

- Validation obligatoire
- Auth + middleware Laravel
- Mots de passe hashés (bcrypt)
- Aucune donnée sensible dans l’API
- Jamais de SQL brut concaténé

---

## Style & Formatage

### Laravel (PHP)
- 4 espaces
- PSR-12
- `'simple quotes'` par défaut

### React (JSX)
- 2 espaces
- Double quotes → " "

---

## Checklist avant commit (Adaptée au projet)

### Backend
- [ ] Routes testées en navigation normale
- [ ] Blade rendu sans erreurs
- [ ] SQL testé et sécurisé
- [ ] Validations bien mises
- [ ] Pas d'ORM → vérification des DB::select / update

### Frontend
- [ ] Composants React fonctionnent dans Blade
- [ ] AJAX correct
- [ ] Pas d’appel vers API REST

### Sécurité
- [ ] Protection CSRF Laravel OK
- [ ] Validation des formulaires
- [ ] Aucune donnée sensible retournée en JSON

---

### Backend
- [ ] Routes API documentées
- [ ] Validation Form Requests
- [ ] Services utilisés pour logique métier
- [ ] Tests de base

### Frontend
- [ ] Composants propres
- [ ] Appels API centralisés
- [ ] Hooks bien nommés

### Sécurité
- [ ] Aucune information sensible dans les réponses
- [ ] Auth correctement configurée

---

## Version & Équipe
- **Version : 1.0 — Décembre 2025**
- **Projet : Babysitter Services – Programmation Web Avancée**
- **Équipe : 4 membres**
