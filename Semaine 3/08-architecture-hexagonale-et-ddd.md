# Architecture Hexagonale et Domain-Driven Design (DDD)

## Objectifs

À la fin de cette section, vous serez capables de :

- Comprendre le rôle du **DDD dans l’architecture hexagonale**.
- Concevoir des **entités métiers riches et cohérentes**.
- Utiliser les **Aggregates, Value Objects et Domain Events**.
- Appliquer une **séparation stricte entre le domaine et l’infrastructure**.
- Structurer une application en **Bounded Contexts** et gérer la communication entre eux.

---

## 1. Introduction au Domain-Driven Design (DDD)

### **Qu’est-ce que le Domain-Driven Design ?**

Le **Domain-Driven Design (DDD)**, proposé par **Eric Evans**, est une **approche de conception logicielle** qui met *
*le domaine métier au centre de l’architecture**.

Dans une approche classique, l’architecture d’une application est souvent orientée autour **de la persistance des
données** (modèle basé sur la base de données), ce qui entraîne une forte dépendance entre les couches et limite
l’évolutivité.

En DDD, l’architecture est pensée **autour des règles métier**, avec une séparation nette entre **le cœur métier** et
les couches externes.

---

### **Pourquoi utiliser le DDD ?**

- Permet d’avoir un **code aligné avec la réalité métier**, en reflétant directement les processus métiers dans le code.
- Sépare clairement **la logique métier de l’infrastructure**, réduisant ainsi le couplage et rendant l’application plus
  évolutive.
- Facilite l’**évolution du projet** en structurant le domaine en plusieurs sous-domaines indépendants.
- Permet une **communication fluide entre développeurs et experts métier**, grâce à un langage commun appelé *
  *Ubiquitous Language**.

---

## 2. Les Principes Clés du DDD

### **1. Bounded Context**

Un **Bounded Context** définit une **limite claire autour d’un sous-domaine** dans un système complexe.  
Chaque contexte possède son **propre modèle métier** et ses **propres règles**, ce qui permet d’éviter les conflits
terminologiques et les dépendances inutiles.

**Pourquoi segmenter une application en plusieurs Bounded Contexts ?**

- **Réduction de la complexité** : chaque partie du domaine est indépendante.
- **Meilleure organisation** : chaque équipe peut travailler sur un contexte spécifique.
- **Facilité d’évolution** : un contexte peut être modifié sans impacter les autres.

---

### **Exemple de Bounded Contexts dans un système e-commerce :**

- **Contexte "Gestion des Produits"** : création, mise à jour des articles, catégories.
- **Contexte "Gestion des Commandes"** : validation d’une commande, suivi de livraison.
- **Contexte "Facturation"** : génération de factures, paiements.
- **Contexte "Gestion des Clients"** : gestion des comptes utilisateurs, fidélité.

Chaque contexte **communique avec les autres via des APIs ou des événements**.

---

### **2. Entités et Value Objects**

| Concept          | Définition                                                      | Exemples                               |
|------------------|-----------------------------------------------------------------|----------------------------------------|
| **Entité**       | Un objet **avec un identifiant unique** et un cycle de vie long | `Utilisateur`, `Commande`, `Produit`   |
| **Value Object** | Un objet **sans identifiant**, immuable                         | `Adresse`, `Devise`, `Coordonnées GPS` |

#### **Pourquoi utiliser des Value Objects ?**

- **Évite les effets de bord** : ils sont immuables, donc pas de modifications involontaires.
- **Réduit la complexité** : pas besoin de gérer un identifiant unique.
- **Améliore la réutilisabilité** : ils peuvent être utilisés dans plusieurs entités.

#### **Exemple : Gestion d’une adresse en tant que Value Object**

Une adresse peut être représentée par un **Value Object**, car elle ne nécessite pas d’identifiant unique et ne change
pas après sa création.

---

### **3. Aggregates et Root Aggregate**

Un **Aggregate** est un **groupe cohérent d’Entités et de Value Objects**, qui garantit **la cohérence des données**.

L’élément principal d’un Aggregate est **l’Aggregate Root**, qui contrôle les accès aux autres objets de l’agrégat.  
Seule **l’Aggregate Root peut être modifiée directement**.

---

#### **Exemple : Gestion d’une commande dans un site e-commerce**

L’**aggregate `Commande`** peut contenir :

- **Entités** : `LigneCommande`, `Produit`
- **Value Objects** : `Adresse de livraison`, `Prix`

#### **Pourquoi utiliser des Aggregates ?**

- **Assure la cohérence métier** : évite les modifications illogiques sur des objets liés.
- **Facilite la gestion des transactions** : toute modification passe par la racine.

**Règle clé :**  
Un **aggregate ne doit pas être trop grand** pour éviter des mises à jour coûteuses en performance.

---

### **4. Domain Events et CQRS**

Un **Domain Event** est un événement métier qui représente un **changement important dans le domaine**.  
Au lieu d’appeler directement des méthodes sur d’autres objets, on publie un événement que d’autres composants peuvent
écouter.

---

#### **Pourquoi utiliser des Domain Events ?**

- **Évite un couplage fort** entre les modules.
- **Facilite la communication entre microservices** dans une architecture distribuée.
- **Permet d’implémenter CQRS** (Command Query Responsibility Segregation) plus efficacement.

#### **Exemple : Gestion d’un événement "Commande Validée"**

1. L’utilisateur passe une commande.
2. L’événement "Commande Validée" est publié.
3. Le service de paiement est notifié et déclenche le paiement.
4. Le service de logistique reçoit l’événement et prépare l’expédition.

Chaque service **réagit indépendamment** à l’événement, ce qui améliore la scalabilité.

---

### **5. CQRS : Séparer les Commandes et les Requêtes**

Le **CQRS (Command Query Responsibility Segregation)** est un modèle où l’on **sépare la lecture et l’écriture** des
données.

#### **Pourquoi utiliser CQRS ?**

- **Améliore les performances** : on optimise différemment les requêtes et les mises à jour.
- **Simplifie la scalabilité** : les lectures et écritures peuvent être déployées séparément.
- **Facilite l’intégration d’Event Sourcing**.

---

### **6. Relation entre DDD et Architecture Hexagonale**

L’architecture hexagonale s’intègre parfaitement avec le **DDD**, car elle impose :

- Une **séparation stricte** entre le domaine et l’infrastructure.
- Un **faible couplage** entre les composants.
- Une **organisation centrée sur le métier**, avec un **domaine autonome**.

---

## Conclusion

Le **Domain-Driven Design** est une approche essentielle pour construire des **applications robustes et évolutives**.  
L’**architecture hexagonale** apporte une structure claire qui permet d’**isoler le domaine des détails techniques**.

Voici un excellent résumé du DDD par **Abel Avram** et **Floyd Marinescu** :

### [Domain-Drive Design vite fait](../Ressources/DDDViteFait.pdf)
