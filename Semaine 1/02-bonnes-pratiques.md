# Bonnes pratiques de développement

## Objectifs

À la fin de cette section, vous serez capables de :

- Appliquer les **principes SOLID** pour une meilleure architecture.
- Utiliser des bonnes pratiques comme **DRY, KISS, YAGNI**.
- Éviter les erreurs de conception grâce à des patterns éprouvés.

---

## Principes SOLID

1. **S**ingle Responsibility Principle (**SRP**)
    - Une classe doit avoir **une seule responsabilité**.  
      **Mauvais exemple :**
      ```php
      class Report {
         public function generateReport() {
            // Génère un rapport
         }
   
         public function saveToFile() {
            // Sauvegarde le rapport dans un fichier
         }
   
         public function sendByEmail() {
            // Envoie le rapport par e-mail
         }
      }
      ```

      **Bon exemple :**
      ```php
      class ReportGenerator {
         public function generateReport() {
            return "Contenu du rapport";
         }
      }

      class ReportSaver {
         public function saveToFile($content) {
            // Sauvegarde le rapport
         }
      }

      class ReportSender {
         public function sendByEmail($content) {
            // Envoi du rapport
         }
      }
      ```

2. **O**pen/Closed Principle (**OCP**)
    - Une classe doit être **ouverte à l'extension mais fermée à la modification**.  
      **Mauvais exemple :**
      ```php
      class Discount {
           public function calculateDiscount($type, $amount) {
               if ($type === 'student') {
                   return $amount * 0.2;
               } elseif ($type === 'vip') {
                    return $amount * 0.3;
               }
               return 0;
           }
      }
      ```

      **Bon exemple :**
      ```php
      interface DiscountStrategy {
         public function calculate($amount);
      }
 
      class StudentDiscount implements DiscountStrategy {
           public function calculate($amount) {
                return $amount * 0.2;
           }
      }
 
      class VipDiscount implements DiscountStrategy {
           public function calculate($amount) {
                return $amount * 0.3;
           }
      }
 
      class DiscountCalculator {
           public function calculateDiscount(DiscountStrategy $strategy, $amount) {
                return $strategy->calculate($amount);
           }
      }
      ```

3. **L**iskov Substitution Principle (**LSP**)
    - Une sous-classe doit pouvoir remplacer sa superclasse sans casser l’application.  
      **Mauvais exemple :**
      ```php
      class Bird {
          public function fly() {
              return "Je vole";
          }
      }

      class Penguin extends Bird {
          public function fly() {
              throw new Exception("Je ne peux pas voler !");
          }
      }
      ```

      **Bon exemple :**
      ```php
      interface Flyable {
          public function fly();
      }

      class Sparrow implements Flyable {
          public function fly() {
              return "Je vole";
          }
      }

      class Penguin {
          public function swim() {
              return "Je nage";
          }
      }
      ```

4. **I**nterface Segregation Principle (**ISP**)
    - Une interface doit être **spécifique et ne pas imposer de méthodes inutiles**.  
      **Mauvais exemple :**
      ```php
      interface Worker {
          public function work();
          public function eat();
      }

      class Robot implements Worker {
          public function work() {
              return "Je travaille";
          }

          public function eat() {
              throw new Exception("Je ne mange pas !");
          }
      }
      ```

      **Bon exemple :**
      ```php
      interface Workable {
          public function work();
      }

      interface Eatable {
          public function eat();
      }

      class Human implements Workable, Eatable {
          public function work() {
              return "Je travaille";
          }

          public function eat() {
              return "Je mange";
          }
      }

      class Robot implements Workable {
            public function work() {
               return "Je travaille";
            }
      }
      ```

5. **D**ependency Inversion Principle (**DIP**)
    - Une classe doit dépendre **d’abstractions et non d’implémentations concrètes**.  
      **Mauvais exemple :**
      ```php
      class MySQLDatabase {
          public function saveOrder($order) {
              // Sauvegarde l'ordre en base MySQL
          }
      }

      class OrderService {
          private $db;

          public function __construct() {
              $this->db = new MySQLDatabase();
          }

          public function placeOrder($order) {
              $this->db->saveOrder($order);
          }
      }
      ```

      **Bon exemple :**
      ```php
      interface Database {
          public function saveOrder($order);
      }

      class MySQLDatabase implements Database {
          public function saveOrder($order) {
              // Sauvegarde en MySQL
          }
      }

      class PostgreSQLDatabase implements Database {
          public function saveOrder($order) {
              // Sauvegarde en PostgreSQL
          }
      }

      class OrderService {
          private $db;

          public function __construct(Database $db) {
              $this->db = $db;
          }

          public function placeOrder($order) {
              $this->db->saveOrder($order);
          }
      }
      ```

---

## Autres bonnes pratiques

- **DRY (Don't Repeat Yourself)** :  
  Éviter la duplication de code. Si vous avez besoin de répéter une logique, créez une fonction ou une classe pour la
  réutiliser.
- **KISS (Keep It Simple, Stupid)** :  
  Garder le code aussi simple que possible. Évitez la complexité inutile. Un code simple est plus facile à comprendre, à
  partager et à maintenir.
- **YAGNI (You Ain’t Gonna Need It)** :  
  Ne pas coder une fonctionnalité inutile. Ajoutez-la uniquement si vous en avez besoin ou si elle est demandée.
- **Separation of Concerns (SoC)** :  
  Séparer clairement les responsabilités du code. Par exemple, ne mélangez pas la logique métier et l’affichage.
  Utilisez des classes ou des fonctions distinctes pour chaque tâche.
- **Design Patterns** :  
  Utilisez des **patterns de conception** éprouvés pour résoudre des problèmes récurrents. Par exemple, le **Factory
  Pattern** pour créer des objets, l’**Observer Pattern** pour gérer des événements, etc.
- **Tests & CI/CD** :  
  Mettez en place des tests unitaires et d’intégration pour valider votre code. Utilisez des outils de **CI/CD** pour
  automatiser les tests et le déploiement.

## De façon générale

- **Documentation** :  
  Commentez votre code pour expliquer son fonctionnement. Utilisez des outils de documentation comme **Doxygen** ou
  **JSDoc** pour générer une documentation automatique.
- **Versioning** :  
  Utilisez un système de **gestion de versions** (Git, SVN) pour suivre les modifications de votre code. Utilisez des
  branches pour travailler sur des fonctionnalités isolées et fusionnez-les ensuite.
- **Code Review** :  
  Faites relire votre code par un pair pour détecter les erreurs, les bugs et les améliorations possibles. Utilisez des
  outils de **code review** comme **GitHub** ou **GitLab** pour faciliter ce processus.
- **Refactoring** :  
  Améliorez régulièrement votre code en le **refactorant**. Simplifiez-le, supprimez les doublons, renommez les
  variables pour le rendre plus lisible et maintenable.
- **Monitoring & Logging** :  
  Surveillez les performances de votre application en mettant en place des outils de **monitoring**. Loggez les
  événements importants pour faciliter le débogage en cas de problème.
- **Security** :  
  Protégez votre application contre les attaques en utilisant des bonnes pratiques de **sécurité**. Évitez les
  injections SQL, les failles XSS, etc.
- **Performance** :  
  Optimisez les performances de votre application en utilisant des **caches**, des **index** en base de données, des
  **requêtes asynchrones**, etc.
- **Error Handling** :  
  Gérez correctement les erreurs en utilisant des **exceptions** ou des **codes d’erreur**. Propagez les erreurs
  correctement pour les traiter au bon niveau.
