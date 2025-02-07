# Microservices : Conception et Communication

## Objectifs

À la fin de cette section, vous serez capables de :

- Comprendre les **principes fondamentaux des microservices**.
- Découper une application en **services autonomes**.
- Gérer la **communication entre microservices** via API et Event-driven.
- Appliquer les bonnes pratiques pour **éviter les pièges courants** liés aux microservices.

---

## 1. Introduction aux Microservices

### **Qu’est-ce qu’une architecture microservices ?**

Une architecture **microservices** consiste à structurer une application en plusieurs **services indépendants**,  
chacun étant **responsable d’un domaine spécifique**.

Contrairement aux **architectures monolithiques**, où toutes les fonctionnalités sont regroupées dans une seule
application,  
les microservices permettent de **déployer, maintenir et faire évoluer chaque composant séparément**.

---

### **Pourquoi utiliser les microservices ?**

#### **Avantages :**

| Avantage                      | Explication                                                                                         |
|-------------------------------|-----------------------------------------------------------------------------------------------------|
| **Scalabilité**               | Chaque microservice peut être mis à l’échelle indépendamment des autres.                            |
| **Flexibilité technologique** | Chaque service peut être développé avec une technologie différente (ex: Node.js, Java, Go…).        |
| **Déploiement indépendant**   | On peut mettre à jour un service sans impacter le reste du système.                                 |
| **Résilience**                | Une panne d’un service n’affecte pas directement les autres, sauf dépendance critique.              |
| **Maintenance facilitée**     | Chaque équipe peut gérer un microservice spécifique, favorisant la spécialisation et la modularité. |

#### **Inconvénients :**

| Inconvénient                           | Explication                                                                                                   |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------|
| **Complexité accrue**                  | Gérer plusieurs services implique une orchestration avancée (API Gateway, monitoring, observabilité…).        |
| **Gestion des transactions difficile** | Les transactions distribuées nécessitent des mécanismes comme **Saga** pour assurer la cohérence des données. |
| **Surcoût en performance**             | Les appels réseau entre microservices sont plus coûteux que des appels internes en mémoire dans un monolithe. |
| **Orchestration nécessaire**           | Besoin d’un API Gateway pour centraliser l'accès aux services et un monitoring efficace.                      |

---

## 2. Découpage des Microservices

Le découpage des microservices doit respecter plusieurs **principes clés** pour éviter de créer une architecture trop
fragmentée (microservices trop petits) ou trop couplée (microservices trop dépendants les uns des autres).

---

### **Comment bien découper ses microservices ?**

Un bon microservice doit :

- **Être autonome** : il ne doit pas dépendre directement d’un autre service pour fonctionner.
- **Correspondre à un domaine métier** : chaque microservice doit refléter une **fonctionnalité métier distincte**.
- **Gérer sa propre base de données** : chaque service doit être indépendant sur le stockage de ses données (principe *
  *Database per Service**).
- **Communiquer uniquement via des APIs ou des événements** : les services ne doivent jamais accéder directement aux
  bases de données des autres.

---

### **Exemple de découpage logique :**

Un **système de gestion de tâches** peut être structuré ainsi :

1. **Service Utilisateur**
    - Gestion des comptes, des rôles et des permissions.
    - Authentification et gestion des accès.

2. **Service Tâches**
    - Création, modification et suppression des tâches.
    - Suivi de l’état des tâches (à faire, en cours, terminé).

3. **Service Facturation**
    - Gestion des paiements, TVA et reçus.
    - Génération des factures et envoi par e-mail.

**Chaque service dispose de sa propre base de données et expose une API REST ou GraphQL**.

---

## 3. Communication entre Microservices

Dans une architecture microservices, les services doivent échanger des informations sans être **directement couplés**.

### **1. API REST et GraphQL**

L’échange d’informations peut se faire via **API HTTP** :

#### **REST**

- **Principe** : chaque service expose une API **RESTful** pour permettre aux autres de récupérer ou envoyer des
  données.
- **Exemple** : Le **Service Tâches** expose une route `/tasks/{id}` pour récupérer une tâche.

#### **GraphQL**

- **Avantage** : Permet aux clients de **spécifier précisément quelles données** ils veulent récupérer.
- **Exemple** : Un frontend peut récupérer les informations **d’une tâche** sans récupérer toutes les données inutiles.

**Limite** : Les API synchrones (REST, GraphQL) nécessitent une haute disponibilité des services.  
Si un service est en panne, la requête échouera.

---

### **2. Événements et Messaging (Event-driven architecture)**

Une alternative aux API REST est **l’architecture orientée événements** (**Event-Driven Architecture**).  
Au lieu de faire des appels directs entre services, un microservice **publie un événement** et les autres services *
*réagissent**.

#### **Pourquoi utiliser la communication asynchrone ?**

- **Évite un couplage fort** entre les services.
- **Améliore la résilience** : les événements sont mis en file d’attente et traités dès qu’un service est disponible.
- **Permet le scaling horizontal** : plusieurs instances d’un même microservice peuvent consommer les événements.

#### **Mise en œuvre**

1. **Un service publie un événement** lorsqu’une action est réalisée.
2. **Les autres services écoutent l’événement** et réagissent en conséquence.

**Exemple :**

1. L’utilisateur passe une commande.
2. L’événement "Commande Validée" est publié sur RabbitMQ/Kafka.
3. Le service de facturation est notifié et génère une facture.
4. Le service de logistique est notifié et prépare l’expédition.

#### **Technologies utilisées pour la communication asynchrone :**

| Outil             | Type                        | Cas d’usage                                              |
|-------------------|-----------------------------|----------------------------------------------------------|
| **RabbitMQ**      | Message broker              | Gestion des files d’attente et priorisation des messages |
| **Apache Kafka**  | Streaming d’événements      | Gestion d’un flux d’événements temps réel                |
| **Redis Pub/Sub** | Système de messagerie léger | Notification rapide entre services                       |

**Inconvénients :**

- La communication **asynchrone est plus difficile à déboguer** qu’une API REST.
- Il faut **éviter la duplication d’événements** (utilisation d’un **Event Store** recommandé).

---

### **3. Patterns avancés de communication**

1. **Orchestration vs Chorégraphie**

- **Orchestration** : Un service central (ex : **Orchestrateur Saga**) gère le flux des interactions.
- **Chorégraphie** : Chaque service **agit en autonomie**, en réagissant aux événements.

2. **Saga Pattern**

- Permet d’assurer la **cohérence des transactions distribuées**.
- Exemple : Un service de paiement doit être annulé si la commande est annulée.

3. **API Gateway**

- Centralise l’accès aux microservices.
- Implémente des fonctionnalités comme **l’authentification, la mise en cache et le monitoring**.

---

## 4. Bonnes pratiques pour éviter les pièges des microservices

| Erreur fréquente                              | Solution recommandée                                                   |
|-----------------------------------------------|------------------------------------------------------------------------|
| **Microservices trop petits**                 | Ne pas fragmenter inutilement, garder un bon équilibre                 |
| **Couplage entre services**                   | Utiliser la communication par événements plutôt que des appels directs |
| **Dépendance à une base de données partagée** | Chaque microservice doit avoir sa propre base de données               |
| **Mauvaise gestion des erreurs**              | Implémenter un système de retries et fallback (ex : Circuit Breaker)   |
| **Manque de monitoring**                      | Utiliser des outils comme Prometheus, Grafana ou OpenTelemetry         |

---

## Conclusion

Les **microservices** permettent de **scaler facilement** et d’**optimiser chaque service indépendamment**.  
Cependant, leur mise en œuvre **nécessite une bonne organisation**, notamment en termes de **communication et
d’orchestration**.
