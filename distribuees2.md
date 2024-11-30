### **Fragmentation dans les Bases de Données Distribuées**

La **fragmentation** est une technique utilisée pour diviser les données d'une base de données en parties plus petites (appelées **fragments**) afin de les stocker sur différents sites ou serveurs. Cela améliore la performance, la scalabilité et la localisation des données dans les bases de données distribuées.

Il existe **deux principaux types de fragmentation** : **horizontale** et **verticale**.

---

### **1. Fragmentation Horizontale**

Dans la **fragmentation horizontale**, les lignes (ou enregistrements) d'une table sont divisées en fragments selon certains critères. Chaque fragment contient un sous-ensemble de lignes.

#### **Caractéristiques :**
- Chaque fragment a les **mêmes colonnes** que la table originale, mais contient **un sous-ensemble des lignes**.
- Les critères de division sont souvent basés sur des valeurs spécifiques, comme une région géographique, un intervalle d’ID, ou d’autres attributs.

#### **Exemple :**
Une table `Clients` contient les colonnes `ID`, `Nom`, et `Région`.

| **ID** | **Nom**  | **Région** |
|--------|----------|------------|
| 1      | Alice    | Europe     |
| 2      | Bob      | Amérique   |
| 3      | Charlie  | Europe     |
| 4      | Diana    | Asie       |

- **Fragment 1 (Europe)** :
  | **ID** | **Nom**  | **Région** |
  |--------|----------|------------|
  | 1      | Alice    | Europe     |
  | 3      | Charlie  | Europe     |

- **Fragment 2 (Amérique)** :
  | **ID** | **Nom**  | **Région** |
  |--------|----------|------------|
  | 2      | Bob      | Amérique   |

- **Fragment 3 (Asie)** :
  | **ID** | **Nom**  | **Région** |
  |--------|----------|------------|
  | 4      | Diana    | Asie       |

#### **Avantages :**
- Améliore les performances en localisant les données proches des utilisateurs concernés.
- Permet un accès rapide aux données pertinentes pour un site donné.

#### **Inconvénients :**
- Les requêtes globales (par exemple, retrouver tous les clients) nécessitent l'accès à plusieurs fragments.

---

### **2. Fragmentation Verticale**

Dans la **fragmentation verticale**, les colonnes d'une table sont divisées en fragments. Chaque fragment contient un sous-ensemble des colonnes, avec une colonne clé pour relier les fragments.

#### **Caractéristiques :**
- Chaque fragment a **un sous-ensemble de colonnes** de la table originale.
- Une colonne clé (généralement l'ID) est incluse dans chaque fragment pour permettre de reconstruire la table originale si nécessaire.

#### **Exemple :**
Une table `Employés` contient les colonnes `ID`, `Nom`, `Adresse`, et `Salaire`.

| **ID** | **Nom**   | **Adresse**       | **Salaire** |
|--------|-----------|-------------------|-------------|
| 1      | Alice     | Paris            | 50,000      |
| 2      | Bob       | New York         | 60,000      |
| 3      | Charlie   | Londres          | 55,000      |

- **Fragment 1 (Identité)** :
  | **ID** | **Nom**   |
  |--------|-----------|
  | 1      | Alice     |
  | 2      | Bob       |
  | 3      | Charlie   |

- **Fragment 2 (Adresse)** :
  | **ID** | **Adresse**       |
  |--------|-------------------|
  | 1      | Paris            |
  | 2      | New York         |
  | 3      | Londres          |

- **Fragment 3 (Salaire)** :
  | **ID** | **Salaire** |
  |--------|-------------|
  | 1      | 50,000      |
  | 2      | 60,000      |
  | 3      | 55,000      |

#### **Avantages :**
- Réduit la taille des fragments, ce qui améliore les performances pour des requêtes spécifiques (par exemple, récupérer uniquement les salaires).
- Gère la confidentialité en stockant certaines colonnes sensibles (comme les salaires) sur des sites sécurisés.

#### **Inconvénients :**
- Les requêtes nécessitant plusieurs colonnes (par exemple, `Nom` et `Salaire`) impliquent une jointure entre fragments, ce qui peut être coûteux.

---

### **3. Combinaison des Deux : Fragmentation Hybride**

Dans certaines situations, on peut combiner les deux types de fragmentation :
- On applique une **fragmentation horizontale** pour diviser les lignes sur des sites différents.
- Ensuite, on applique une **fragmentation verticale** pour diviser les colonnes au sein de chaque fragment horizontal.

---

### **Comparaison entre Fragmentation Horizontale et Verticale**

| **Critère**            | **Fragmentation Horizontale**                  | **Fragmentation Verticale**                   |
|-------------------------|-----------------------------------------------|-----------------------------------------------|
| **Unités fragmentées**  | Lignes de la table                            | Colonnes de la table                          |
| **Structure des fragments** | Chaque fragment a les mêmes colonnes       | Chaque fragment a des colonnes différentes    |
| **Utilisation typique** | Localisation géographique, segmentation par région | Confidentialité des données, optimisation des requêtes |
| **Requêtes complexes**  | Nécessitent un accès à plusieurs fragments    | Nécessitent des jointures entre fragments     |

---

### **Résumé Simplifié**
- **Fragmentation horizontale** : Les **lignes** sont divisées en fragments basés sur des critères comme la région ou l'ID.
  - Idéal pour localiser les données proches des utilisateurs.
- **Fragmentation verticale** : Les **colonnes** sont divisées en fragments, en incluant une clé pour relier les données.
  - Utile pour réduire la taille des fragments ou protéger certaines données sensibles.
