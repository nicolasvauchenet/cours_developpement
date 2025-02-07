# Concepts clés de la Clean Architecture

## Objectifs

À la fin de cette section, vous serez capables de :

- Expliquer le rôle des **Use Cases, DTOs, Ports & Adapters**.
- Comprendre comment **l'inversion des dépendances** améliore la structure du code.
- Définir une architecture modulaire respectant ces principes.

---

## 1. Inversion des Dépendances

### **Problème des dépendances classiques**

- Dans une architecture traditionnelle, **les couches hautes dépendent des couches basses** (ex : un service dépend d’un
  repository).
- Si on change le système de stockage (ex : MySQL → MongoDB), on doit **modifier le service et le repository**.

### **Solution : Inversion des dépendances**

En Clean Architecture, **la logique métier ne dépend de rien**.

- **Les dépendances doivent toujours pointer vers le domaine, jamais vers l’infrastructure.**
- On utilise **des interfaces** pour découpler les interactions entre les couches.

---

## 2. Use Cases et Services

### **Définition des Use Cases**

- Un **Use Case** représente un **cas d’utilisation métier spécifique**.
- Il encapsule une **règle métier complète** sans dépendre des détails techniques.

**Exemple de Use Cases :**

- `Créer une tâche`
- `Modifier le statut d’une tâche`
- `Assigner une tâche à un utilisateur`

Chaque **Use Case** est indépendant et peut être **testé séparément**.

---

## 3. DTOs (Data Transfer Objects)

### **Pourquoi utiliser des DTOs ?**

- **Un DTO est un objet qui transporte des données** entre couches sans contenir de logique métier.
- Il permet d’**éviter d’exposer directement les entités** de la couche Domaine.

**Avantages des DTOs :**

- **Réduit le couplage** entre les couches.
- **Simplifie le transport des données**.
- **Sécurise l'accès aux données sensibles**.

---

## 4. Différences entre Clean Architecture et Architecture Hexagonale

### **Pourquoi comparer ces deux architectures ?**

- **Clean Architecture** et **l’Architecture Hexagonale** visent toutes deux à **rendre le code modulaire, testable et
  indépendant des détails techniques**.
- Elles partagent **des principes communs**, notamment :
    - **L’inversion des dépendances**.
    - **La séparation stricte de la logique métier et de l’infrastructure**.
    - **L’indépendance du framework et de la base de données**.

Cependant, **elles diffèrent dans leur approche et leur organisation**.

---

### 🔹 **1. Différences principales**

| **Aspect**              | **Architecture Hexagonale** (Alistair Cockburn)             | **Clean Architecture** (Uncle Bob)              |
|-------------------------|-------------------------------------------------------------|-------------------------------------------------|
| **Organisation**        | Basée sur le modèle **Ports & Adapters**                    | Organisation en **couches concentriques**       |
| **Principe central**    | Découplage entre le cœur métier et le monde extérieur       | Séparation stricte en **cercles**               |
| **Dépendances**         | **Unidirectionnelles** (toujours vers le domaine)           | **Les dépendances pointent vers le domaine**    |
| **Couche métier**       | Entités + Services métiers                                  | Entités + Use Cases                             |
| **Interface technique** | Définie par des **Ports**                                   | Utilise des **Interfaces classiques**           |
| **Adaptabilité**        | Pensée pour être totalement **agnostique** à la technologie | Flexibilité, mais plus orientée **cas d’usage** |

---

### 🔹 **2. Explication visuelle**

#### **Clean Architecture (organisation en cercles)**

```plaintext
+---------------------------+
| Interface utilisateur     |
| (Controllers, Views)      |
+---------------------------+
             ↓
+---------------------------+
|   Cas d'utilisation       |
| (Services, Use Cases)     |
+---------------------------+
             ↓
+---------------------------+
|  Domaine (Business)       |
| (Entities, Rules)         |
+---------------------------+
             ↓
+---------------------------+
|  Infrastructure (DB, API) |
| (Repositories, Adapters)  |
+---------------------------+
```

📌 **Les dépendances pointent toujours vers l’intérieur**, donc :

- La **logique métier ne dépend pas du framework**.
- Les **Use Cases orchestrent les actions** et ne dépendent pas de la persistance.

---

#### **Architecture Hexagonale (Ports & Adapters)**

```plaintext
+------------------------+
|        API REST        |
|     Interface CLI      |
|       Web UI           |
+------------------------+
           ↓
+------------------------+
|         Ports          |
+------------------------+
           ↓
+------------------------+
|    Domaine Métier      |
+------------------------+
           ↓
+------------------------+
|   Adapters (DB, API)   |
+------------------------+
```

📌 **Ici, les Ports définissent comment le domaine interagit avec l’extérieur** :

- L’application expose **des Ports**, qui sont ensuite reliés à des **Adapters**.
- Un **Port** est une **interface** qui définit **comment un service peut interagir avec l’extérieur**.
- Un **Adapter** est une **implémentation concrète** qui permet d’accéder à une **base de données, une API ou un service
  externe**.

---

### 🔹 **3. Quand choisir Clean Architecture ou Hexagonale ?**

| **Critère**                                          | **Hexagonale**                                                  | **Clean Architecture**                                    |
|------------------------------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------|
| **Besoin d’un découplage strict des technologies ?** | ✅ Oui (Ports & Adapters garantissent une indépendance totale)   | ⚠️ Partiel (on peut encore dépendre de Symfony ou NestJS) |
| **Focus sur les cas d’utilisation ?**                | ⚠️ Moyen (elle met plus l’accent sur les interactions externes) | ✅ Oui (Use Cases sont au cœur du modèle)                  |
| **Facilité de mise en place ?**                      | ❌ Complexe (beaucoup d’abstraction et d’interfaces)             | ✅ Plus simple (Use Cases + Interfaces suffisent)          |
| **Adapté aux systèmes distribués ?**                 | ✅ Oui (parfait pour les microservices, CQRS, Event Sourcing)    | ⚠️ Oui, mais pas pensé pour ça à la base                  |

📌 **Résumé** :

- Si on veut un **découplage maximal entre le domaine et les technologies**, **l’Architecture Hexagonale est plus
  adaptée**.
- Si on veut une **organisation autour des cas d’utilisation**, **la Clean Architecture est idéale**.

---

## 5. Ports & Adapters

### **Principe des Ports & Adapters**

- Un **Port** est une **interface** définissant comment un service peut interagir avec le reste de l’application.
- Un **Adapter** est une **implémentation concrète** qui se connecte à une base de données ou à une API.

**Pourquoi utiliser Ports & Adapters ?**

- Permet d’**isoler l’application de l’infrastructure**.
- Facilite le **remplacement d’une technologie** sans impacter la logique métier.

**Les Ports & Adapters sont un concept clé de l’architecture Hexagonale, mais on peut les utiliser dans Clean
Architecture lorsqu’on veut un découplage fort entre les services.**
