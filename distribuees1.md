### **Cours : Différences entre Multibases et Bases de Données Distribuées**

---

#### **Introduction**
Dans le domaine des bases de données, il existe des concepts clés à comprendre : les **multibases** et les **bases de données distribuées**. Bien qu'ils impliquent tous deux l'utilisation de plusieurs sources de données, ils fonctionnent différemment.

---

### **1. Multibases**
Les **multibases** désignent une situation où une application interagit avec plusieurs bases de données **indépendantes**.

#### **Caractéristiques :**
- Chaque base est **indépendante** et peut être différente (PostgreSQL, MySQL, MongoDB, etc.).
- Le programme doit se connecter explicitement à chaque base, avec des configurations spécifiques (hôte, utilisateur, mot de passe, etc.).
- **Aucune coordination native** entre les bases.
- La gestion des connexions et des données est entièrement à la charge de l'application.

#### **Exemple de Multibases :**
Un programme en Python connecte à plusieurs bases :

```python
# Connexion à la base 1
conn1 = mysql.connector.connect(
    host='192.168.1.44',
    user='achrafkdr',
    database='firstdb',
    password='pass1'
)

# Connexion à la base 2
conn2 = mysql.connector.connect(
    host='192.168.1.2',
    user='achrafkdr',
    database='seconddb',
    password='pass2'
)
```

- Chaque base de données est gérée séparément, sans lien direct.

---

### **2. Bases de Données Distribuées**
Une **base de données distribuée** est une **seule base logique** dont les données sont **réparties sur plusieurs sites ou machines**. Cependant, pour l'utilisateur, elle reste une base unique.

#### **Caractéristiques :**
- Les données sont **fragmentées** (partitionnées) ou **répliquées** entre plusieurs sites.
- Un utilisateur se connecte à un site, mais peut accéder aux données des autres sites **sans changer sa connexion**.
- La coordination et la synchronisation des données sont gérées par le système de base de données.
- Transparence : l’utilisateur ne sait pas où les données sont physiquement stockées.

#### **Exemple de Base Distribuée :**
Un système MySQL distribué avec deux sites :
- **Site 1** : 192.168.1.44
- **Site 2** : 192.168.1.2

Avec une base distribuée, si vous vous connectez à **site 1**, vous pouvez accéder aux données de **site 2** sans établir une nouvelle connexion :

```python
# Connexion au site 1
conn = mysql.connector.connect(
    host='192.168.1.44',
    user='achrafkdr',
    database='distributeddb',
    password='pass1'
)

# Requête SQL
cursor = conn.cursor()
cursor.execute("SELECT * FROM table_qui_peut_etre_sur_site_2")
```

Le système MySQL distribué ira chercher les données sur le **site 2** si nécessaire, sans intervention manuelle.

---

### **3. Différences entre Multibases et Bases Distribuées**

| **Caractéristique**            | **Multibases**                                    | **Bases Distribuées**                            |
|---------------------------------|--------------------------------------------------|-------------------------------------------------|
| **Nature des bases**            | Plusieurs bases indépendantes                    | Une seule base logique                          |
| **Coordination**                | Aucune coordination native                       | Gérée par le système de base de données         |
| **Transparence**                | L'utilisateur gère chaque base individuellement  | Vue unifiée pour l'utilisateur                  |
| **Utilisation typique**         | Applications accédant à plusieurs bases hétérogènes | Applications nécessitant scalabilité et disponibilité |
| **Exemple**                     | Connexion à plusieurs bases MySQL distinctes     | Base MySQL distribuée sur plusieurs sites       |

---

### **4. Résumé Simplifié**
- **Multibases** : Plusieurs bases de données séparées, que l'application connecte individuellement.
- **Base distribuée** : Une seule base logique, dont les données sont réparties sur plusieurs sites, mais accessibles de manière unifiée.

#### **Question courante :**
> **Si je me connecte à un site d'une base distribuée, puis-je accéder aux données d'autres sites ?**

**Réponse** : Oui, c'est possible. Une connexion à un site d'une base distribuée donne accès à toutes les données, où qu'elles soient stockées. Le système gère tout cela en arrière-plan.

---
