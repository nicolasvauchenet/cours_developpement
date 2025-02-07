# Interfaces et Modularité

## Objectifs

À la fin de cette section, vous serez capables de :

- Comprendre le rôle des **interfaces** dans une architecture modulaire.
- Concevoir une **architecture flexible et évolutive** grâce aux interfaces.
- Appliquer le principe **d’inversion des dépendances** via les interfaces.
- Structurer des **modules indépendants** pour une meilleure scalabilité.

---

## 1. Pourquoi utiliser des interfaces ?

### **Définition**

Une **interface** définit un **contrat** que doivent respecter les classes qui l’implémentent.  
Elle permet d’assurer que **plusieurs implémentations respectent une structure commune**, sans imposer de logique
métier.

### **Pourquoi sont-elles essentielles en Clean Architecture ?**

Dans une architecture modulaire, les interfaces permettent de :

- **Découpler les couches** en imposant une séparation stricte entre la logique métier et l’infrastructure.
- **Faciliter les tests** en permettant le remplacement des dépendances (ex: repository fictif pour les tests
  unitaires).
- **Permettre l’évolution du système** sans impacter le code métier.

### **Avantages des interfaces**

| **Avantage**                  | **Explication**                                                      |
|-------------------------------|----------------------------------------------------------------------|
| **Modularité**                | Permet de changer une implémentation sans affecter le reste du code. |
| **Inversion des dépendances** | Les classes haut niveau ne dépendent plus des classes bas niveau.    |
| **Facilité des tests**        | Remplacement des implémentations réelles par des **mocks**.          |
| **Extensibilité**             | Ajout de nouvelles fonctionnalités sans modifier le code existant.   |

---

## 2. Architecture modulaire et flexibilité

### **Principes d'une architecture modulaire**

Une **architecture modulaire** vise à organiser l’application sous forme de **modules indépendants** qui peuvent évoluer
sans dépendre les uns des autres.

**Principes fondamentaux :**

1. **Séparer la logique métier de l’infrastructure**
    - Les entités et règles métiers ne doivent **jamais** dépendre des technologies utilisées (ex: base de données).

2. **Utiliser des interfaces pour définir les interactions**
    - La communication entre les modules doit **passer par des interfaces** et non par des dépendances directes.

3. **Encapsuler les services dans des modules indépendants**
    - Chaque module **doit être autonome**, avec ses propres responsabilités et ses propres interfaces.

---

## 3. Modèle modulaire vs Monolithique

**Comparaison entre une architecture modulaire et une architecture monolithique :**

| **Critère**     | **Architecture Monolithique**                         | **Architecture Modulaire**                             |
|-----------------|-------------------------------------------------------|--------------------------------------------------------|
| **Dépendances** | Fort couplage entre les composants                    | Faible couplage, favorise l’indépendance               |
| **Évolutivité** | Complexe : un changement impacte plusieurs parties    | Facile : chaque module évolue indépendamment           |
| **Testabilité** | Difficile, car tous les composants sont liés          | Facile, car chaque module peut être testé isolément    |
| **Scalabilité** | Difficile : toute l’application doit être mise à jour | Facile : on peut déployer indépendamment chaque module |

---

## 4. Application des interfaces en architecture modulaire

### **Comment structurer un module indépendant ?**

Dans une architecture modulaire bien conçue, chaque module suit ce schéma :

```plaintext
+----------------------+
|      Interface       |  ← Définition des contrats (Ports)
+----------------------+
           ↓
+----------------------+
|    Implémentation    |  ← Contient la logique métier ou l’accès aux données
+----------------------+
```

**Exemple d’application dans Clean Architecture** :

- **Un service de gestion des tâches** ne communique pas directement avec la base de données.
- Il utilise une **interface `TaskRepository`**, qui sera implémentée par **une classe spécifique (`TaskRepositorySQL`)
  **.

---

## 5. Modularité et microservices

### **Les interfaces dans une architecture microservices**

- Les **interfaces permettent aux services de communiquer sans dépendances fortes**.
- Un microservice expose souvent **des APIs ou des events**, qui sont des **interfaces contractuelles**.
- Chaque service peut être **remplacé ou mis à jour sans casser les autres**.

**Exemple :**

- Un **microservice de paiement** expose une **interface `PaymentGateway`**.
- Plusieurs implémentations existent (`StripeAdapter`, `PaypalAdapter`), permettant de changer de fournisseur sans
  impacter le service.

---

## **Résumé**

| **Concept**                     | **Explication**                                                                 |
|---------------------------------|---------------------------------------------------------------------------------|
| **Interfaces**                  | Définissent des **contrats** entre modules sans imposer d’implémentation.       |
| **Modularité**                  | Permet d’avoir des composants **indépendants** et facilement testables.         |
| **Inversion des dépendances**   | Les **dépendances pointent vers les interfaces**, pas vers les implémentations. |
| **Microservices et interfaces** | Utilisation d’APIs et de contrats pour découpler les services.                  |
