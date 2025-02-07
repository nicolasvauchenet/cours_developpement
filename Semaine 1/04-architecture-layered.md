# L’Architecture Layered (N-Tiers)

## Objectifs

À la fin de cette section, vous serez capables de :

- Comprendre les principes de l’**architecture en couches (Layered Architecture)**.
- Expliquer le rôle de chaque couche et son interaction avec les autres.
- Identifier les **avantages** et **inconvénients** de cette architecture.

---

## 1. Introduction à l'Architecture Layered

### **Définition**

L’**architecture en couches (Layered Architecture)** est un modèle architectural qui divise une application en **couches
distinctes**, chacune ayant une **responsabilité bien définie**.

Cette séparation permet d’organiser le code de manière plus **modulaire**, ce qui facilite la **maintenance**, l’*
*évolutivité** et la **testabilité** de l’application.

---

### **Pourquoi structurer une application en couches ?**

L’objectif principal de cette architecture est de **séparer les responsabilités** afin d’éviter un code monolithique
difficile à modifier. Elle permet notamment de :

- **Rendre le code plus lisible et compréhensible**
- **Améliorer la maintenabilité** en limitant les effets de bord lors des modifications
- **Favoriser la réutilisation du code** grâce à des composants bien isolés
- **Rendre l’application plus testable** en permettant des tests indépendants sur chaque couche

---

## 2. Structure d’une application Layered

### **Principe général**

Une application en couches est généralement divisée en **quatre couches principales** :

```plaintext
+---------------------------+
| Couche Présentation       | → Interface utilisateur (UI)
|  (Controllers, Views)     |
+---------------------------+
             ↓
+---------------------------+
| Couche Application        | → Logique métier
|  (Services, Use Cases)    |
+---------------------------+
             ↓
+---------------------------+
| Couche Domaine            | → Modèles métiers et entités
|  (Entities, Models)       |
+---------------------------+
             ↓
+---------------------------+
| Couche Infrastructure     | → Accès aux bases de données, API externes
|  (Repositories, DB, APIs) |
+---------------------------+
```

---

### **Rôle de chaque couche**

Chaque couche a une **responsabilité spécifique** et communique uniquement avec la couche immédiatement inférieure ou
supérieure.

1. **Couche Présentation (UI Layer)**
    - Représente l’interface utilisateur et gère l’affichage.
    - Contient généralement des **contrôleurs** et des **vues**.
    - Ne contient **aucune logique métier**. Son seul rôle est de **transmettre les requêtes de l’utilisateur à la
      couche Application**.

2. **Couche Application (Service Layer)**
    - Contient la **logique métier** et les **cas d’utilisation**.
    - C’est ici que sont définies les **règles métiers**, telles que la validation des données ou l’exécution des
      traitements.
    - Ne dépend **ni de la base de données ni de l’interface utilisateur**, ce qui permet une grande flexibilité.

3. **Couche Domaine (Domain Layer)**
    - Contient les **modèles et entités métiers**.
    - C’est l’endroit où l’on définit **les objets métiers et leurs règles internes**.
    - Cette couche est **totalement indépendante du framework ou de l’outil de stockage** utilisé.

4. **Couche Infrastructure (Infrastructure Layer)**
    - Gère **l’accès aux ressources externes** : base de données, API tierces, services externes.
    - Contient des **repositories** et des gestionnaires de persistance.
    - Permet de **découpler la logique métier du stockage des données**, rendant l’application plus flexible et
      portable.

---

## 3. Fonctionnement et interaction entre les couches

### **Flux de communication**

Dans une application Layered, les couches interagissent de manière **hiérarchique**.

**Exemple de traitement d'une requête utilisateur :**

1. L’utilisateur envoie une **requête** via l’interface (Couche Présentation).
2. Le contrôleur transmet la requête au **Service concerné** (Couche Application).
3. Le service applique la **logique métier** et demande des données à la couche Domaine.
4. Si besoin, la couche Domaine récupère des informations depuis la **base de données** via la couche Infrastructure.
5. La réponse suit le chemin inverse et est finalement affichée à l’utilisateur.

---

## 4. Avantages et inconvénients

### **Avantages de l’architecture en couches**

| Avantages                                 | Explication                                                                                                       |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Séparation claire des responsabilités** | Chaque couche a un rôle précis, évitant le couplage excessif.                                                     |
| **Code plus facile à maintenir**          | Modifier une couche n’impacte pas directement les autres.                                                         |
| **Facilite les tests unitaires**          | Chaque couche peut être testée indépendamment.                                                                    |
| **Réutilisation du code**                 | La couche Application et la couche Domaine peuvent être utilisées dans plusieurs interfaces (ex : web et mobile). |
| **Flexibilité accrue**                    | Possibilité de changer la base de données ou l’interface utilisateur sans modifier toute l’application.           |

---

### **Inconvénients de l’architecture en couches**

| Inconvénients                        | Explication                                                              |
|--------------------------------------|--------------------------------------------------------------------------|
| **Complexité accrue au départ**      | Nécessite une bonne compréhension des différentes couches.               |
| **Possibilité de surcouche inutile** | Parfois, des couches peuvent sembler inutiles dans de petits projets.    |
| **Risque de dépendances mal gérées** | Mauvaise implémentation des couches peut créer des dépendances inutiles. |

---

## 5. Comparaison avec d’autres architectures

L’architecture Layered est **une des plus populaires**, mais elle n’est pas toujours adaptée à tous les projets.

| Architecture           | Avantages                                      | Inconvénients                         | Cas d’usage                           |
|------------------------|------------------------------------------------|---------------------------------------|---------------------------------------|
| **Layered**            | Séparation claire des responsabilités          | Complexité pour petits projets        | Applications web classiques           |
| **Clean Architecture** | Très modulaire et testable                     | Plus difficile à comprendre           | Projets évolutifs et modulaires       |
| **Hexagonale**         | Faible couplage avec les technologies externes | Nécessite un bon design initial       | Applications complexes et distribuées |
| **Microservices**      | Très scalable et indépendant                   | Complexité et orchestration difficile | Applications distribuées et scalables |

---

## **Résumé**

| Concept                                     | Explication                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| **Architecture Layered**                    | Division en plusieurs couches indépendantes pour une meilleure modularité.                  |
| **Rôle de chaque couche**                   | UI → Application → Domaine → Infrastructure.                                                |
| **Avantages**                               | Facilité de maintenance, réutilisation, testabilité.                                        |
| **Inconvénients**                           | Complexité accrue, risque de surcouche inutile.                                             |
| **Comparaison avec d’autres architectures** | Moins flexible que la Clean Architecture ou Hexagonale, mais plus simple à mettre en place. |
