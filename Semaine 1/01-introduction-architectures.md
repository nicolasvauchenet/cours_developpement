# Introduction aux architectures logicielles

## Objectifs

À la fin de cette section, vous serez capables de :

- Comprendre pourquoi structurer une application est essentiel.
- Différencier un **monolithe** d’une **architecture en couches**.
- Identifier les **avantages** et **inconvénients** de ces architectures.

---

## Pourquoi structurer une application ?

Un code bien structuré apporte plusieurs avantages :

- **Lisibilité** : Plus facile à comprendre et maintenir.
- **Réutilisabilité** : Certains composants peuvent être utilisés ailleurs.
- **Évolutivité** : Facilite l'ajout de nouvelles fonctionnalités.
- **Testabilité** : Chaque partie peut être testée indépendamment.

---

## Monolithes vs architectures en couches

### **Monolithe**

Un **monolithe** est une application où toutes les fonctionnalités sont regroupées sans réelle séparation.

➡ **Avantages** : Développement rapide, simple au départ.  
➡ **Inconvénients** : Maintenance difficile, couplage fort, peu évolutif.

### **Architecture en couches (Layered Architecture)**

Cette architecture divise une application en **plusieurs niveaux indépendants** :

1. **Couche Présentation (UI)**  
   La partie visible de l'application (interface utilisateur). C'est ici que l'utilisateur interagit, via un navigateur
   ou une application mobile. Dans cette couche, nous retrouvons principalement les **contrôleurs** et les **vues**.

2. **Couche Application (Service)**  
   La logique métier de l'application. Elle traite les données reçues de la couche Présentation et les transmet à la
   couche Domaine. On y trouve les **services** et les **DTOs** (Data Transfer Objects).

3. **Couche Domaine (Entités, Modèles)**  
   La couche métier de l'application. Elle contient les **entités** (objets métier) et les **repositories** (accès aux
   données). C'est ici que se trouvent les règles de gestion.

4. **Couche Infrastructure (Base de données, APIs externes)**  
   La couche technique de l'application. Elle gère les accès aux données (base de données, APIs externes) et les
   détails d'implémentation. On y trouve les **repositories**, les **clients HTTP**, etc. Cette couche est intiment
   liée aux détails techniques de l'application (base de données, APIs externes, etc.).

- **Avantages** : Modulaire, maintenable, évolutif.
- **Inconvénients** : Complexité accrue au début.
