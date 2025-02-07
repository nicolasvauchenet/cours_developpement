# Gestion des Dépendances et API Gateway

## Objectifs

À la fin de cette section, vous serez capables de :

- Comprendre le rôle d’une **API Gateway** dans une architecture microservices.
- Gérer les dépendances entre services de manière efficace.
- Assurer la **scalabilité et la résilience** d’une application distribuée.
- Appliquer les bonnes pratiques pour **éviter les problèmes courants liés aux microservices**.

---

## 1. Pourquoi une API Gateway ?

### **Définition et rôle d’une API Gateway**

Une **API Gateway** est un **point d’entrée unique** pour toutes les requêtes externes vers une architecture
microservices.

Dans un système microservices classique :

- Les **clients (frontend, applications mobiles, partenaires)** doivent souvent **interagir avec plusieurs services**
  pour récupérer des données.
- Sans API Gateway, chaque client doit **gérer lui-même** les appels aux différents services, ce qui **complexifie la
  gestion** et réduit la performance.

L’**API Gateway** agit comme un **intermédiaire** entre les clients et les microservices :

- Elle **centralise l’authentification et la gestion des accès**.
- Elle **optimise le trafic** en agrégeant plusieurs requêtes en une seule.
- Elle **améliore la sécurité** en empêchant l’exposition directe des services internes.

---

### **Pourquoi utiliser une API Gateway ?**

| Avantage                          | Explication                                                                            |
|-----------------------------------|----------------------------------------------------------------------------------------|
| **Réduction du couplage**         | Les clients n’ont plus besoin de connaître la structure interne des microservices.     |
| **Authentification centralisée**  | L’API Gateway peut gérer **OAuth2, JWT, API Keys** et déléguer l’accès aux services.   |
| **Optimisation des performances** | Mise en cache, compression des réponses, agrégation des appels API.                    |
| **Sécurité renforcée**            | Filtrage des requêtes, protection contre les attaques **DDoS**, gestion des quotas.    |
| **Gestion des versions d’API**    | Permet d’exposer plusieurs versions d’une API sans impacter les services sous-jacents. |
| **Monitoring et journalisation**  | Centralisation des logs et métriques sur les requêtes entrantes.                       |

**Exemple concret** :

- Un client mobile veut récupérer un **profil utilisateur** et la **liste des tâches**.
- **Sans API Gateway** : il doit appeler **deux microservices séparés** (`/users/{id}` et `/tasks/{user_id}`).
- **Avec API Gateway** : il appelle une **seule route** `/api/profile/{id}`, qui **récupère et agrège les données**
  avant de les retourner.

---

## 2. Outils et implémentation d’une API Gateway

### **Exemples d’outils API Gateway**

| Outil               | Type                | Fonctionnalités principales                                                   |
|---------------------|---------------------|-------------------------------------------------------------------------------|
| **Kong**            | Open-source         | Authentification, monitoring, mise en cache, plugins extensibles.             |
| **Traefik**         | Open-source         | Routage dynamique, support de Docker/Kubernetes, gestion des certificats SSL. |
| **Nginx**           | Open-source         | Reverse proxy performant, load balancing, sécurisation des API.               |
| **AWS API Gateway** | Service cloud       | Gestion des API serverless, intégration Lambda, analytics.                    |
| **Apigee (Google)** | SaaS/API Management | Sécurité, analytics avancées, monétisation d’API.                             |

---

## 3. Gestion des dépendances entre microservices

L’un des **plus grands défis** dans une architecture microservices est de **gérer efficacement les dépendances** entre
les services.

### **1. Comment éviter un couplage excessif entre microservices ?**

- **Favoriser la communication asynchrone** via des événements plutôt que des appels synchrones (REST).
- **Limiter les dépendances directes** en utilisant des interfaces et des abstractions.
- **Utiliser des API stables et versionnées** pour éviter des mises à jour simultanées sur tous les services.

### **2. Database per Service : chaque service doit-il avoir sa propre base de données ?**

- **Principe** : Chaque microservice **doit gérer ses propres données**, sans accès direct aux bases des autres.
- **Avantage** : Évite les conflits d’accès et améliore l’indépendance des services.
- **Solution** : Pour partager des données, utiliser **des événements métiers ou une API centralisée**.

**Problème courant :**

- Le **Service Facturation** a besoin des prix des produits, mais ces données sont gérées par le **Service Produits**.
- **Mauvaise solution** : Interroger directement la base de données du **Service Produits** (fort couplage).
- **Bonne solution** : Le **Service Produits** publie des **événements** ("Changement de prix d’un produit"), que le *
  *Service Facturation** écoute et stocke en cache.

---

## 4. Stratégies pour améliorer la scalabilité et la résilience

Les microservices doivent être **scalables** (supporter un grand nombre d’utilisateurs) et **résilients** (continuer à
fonctionner malgré des pannes).

### **1. Mise en cache et optimisation des requêtes**

- **Cache côté API Gateway** : Évite d’appeler un microservice si la donnée est déjà stockée.
- **Redis/Memcached** : Stocker des réponses temporaires pour réduire la charge sur les bases de données.
- **Compression des réponses** : Réduction de la taille des réponses pour accélérer les requêtes.

### **2. Gestion des pannes et résilience**

| Problème                        | Solution                                                                                     |
|---------------------------------|----------------------------------------------------------------------------------------------|
| **Un service est en panne**     | Utiliser un **fallback** (renvoyer une valeur par défaut).                                   |
| **Temps de réponse trop long**  | Implémenter un **timeout** pour éviter qu’une requête ne bloque indéfiniment.                |
| **Surcharge d’un microservice** | Appliquer un **circuit breaker** pour couper temporairement l’accès à un service défaillant. |

### **3. Load Balancing et Auto-scaling**

- **Load Balancing** : Répartir le trafic entre plusieurs instances d’un service.
- **Auto-scaling** : Augmenter automatiquement le nombre d’instances en fonction de la charge.

---

## 5. Sécurisation des API et gestion des accès

Un système microservices doit être **sécurisé**, car chaque service peut être une porte d’entrée pour des attaques.

### **1. Authentification et gestion des tokens**

- **JWT (JSON Web Tokens)** : Utilisé pour **authentifier les utilisateurs et transmettre des droits d’accès**.
- **OAuth2/OpenID Connect** : Permet une gestion avancée des permissions.

### **2. Protection contre les attaques**

| Problème                        | Solution                                                                |
|---------------------------------|-------------------------------------------------------------------------|
| **Attaques DDoS**               | Limitation du nombre de requêtes (Rate Limiting).                       |
| **Injection SQL/XSS**           | Validation des entrées utilisateurs et pare-feu applicatif (WAF).       |
| **Exposition des API internes** | Ne pas exposer directement les microservices (API Gateway obligatoire). |

### **3. Audit et monitoring**

- **Centralisation des logs** : Utilisation d’outils comme **ELK Stack (Elasticsearch, Logstash, Kibana)**.
- **Monitoring en temps réel** : Prometheus + Grafana pour suivre les performances et les erreurs.

---

## Conclusion

Une **API Gateway** est essentielle pour **centraliser, sécuriser et optimiser** l’accès aux microservices.  
La **gestion des dépendances** doit être pensée dès la conception pour éviter un **couplage excessif**.

En appliquant les **bonnes pratiques de scalabilité et de résilience**, une architecture microservices devient **fiable,
performante et facile à maintenir**.
