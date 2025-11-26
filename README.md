# 🗄️ ITManagementDB - Base de données de gestion du recyclage IT

## 📊 Description

Système de gestion complet pour le tracking des produits IT retournés, leur vérification technique et leur destination finale (recyclage, revente, don).

**Base de données** : PostgreSQL  
**Statut** : ✅ Créée dans pgAdmin  
**Nom** : `ITManagementDB`

---

## 🗂️ Structure du projet

```md
DB_IT_Product_Management/
├── scripts/
│   ├── create_database.sql           # Documentation de la configuration DB
│   ├── seed_data.sql                 # Données de test et exemples
│   └── migrations/
│       ├── 001_initial_schema.sql    # Création de toutes les tables
│       └── 002_add_indexes.sql       # Index, triggers et optimisations
├── queries/
│   ├── reports.sql                   # Requêtes de reporting standards
│   └── analytics.sql                 # Analyses et statistiques avancées
└── assets/                           # Ressources du projet (anciennement "ressources")
    └── DB_modelisation/              # Dossier de modélisation de base de données
        └── modelDbReturnedProduct.drawio  # Diagramme ER complet
```

---

## 🚀 Installation

### ✅ 1. Base de données (déjà créée)

La base `ITManagementDB` a été créée dans **pgAdmin** avec la configuration suivante :

- **Owner** : `postgres`
- **Encoding** : `UTF8`
- **Collation** : `C.UTF-8` (ou `fr_FR.UTF-8`)
- **Connection Limit** : `-1` (illimité)

### 📝 Commande SQL équivalente (pour référence)

```html
<details>
    <summary>Cliquez pour afficher le code SQL</summary>


```sql
CREATE DATABASE "ITManagementDB"
    WITH 
    OWNER = postgres
    ENCODING = 'UTF8'
    LC_COLLATE = 'C.UTF-8'
    LC_CTYPE = 'C.UTF-8'
    TABLESPACE = pg_default
    CONNECTION LIMIT = -1
    IS_TEMPLATE = False;
```

</details>

---

### 📦 2. Exécuter les migrations

#### Option A : Depuis VS Code (recommandé)

**Avec l'extension PostgreSQL :**

1. Ouvrez VS Code et connectez-vous à `ITManagementDB`
2. Ouvrez chaque fichier et exécutez avec `F5` dans l'ordre suivant :

```md
1️⃣ scripts/migrations/001_initial_schema.sql
2️⃣ scripts/migrations/002_add_indexes.sql
3️⃣ scripts/seed_data.sql (optionnel - données de test)
```

#### Option B : Via terminal/ligne de commande

```bash
# Naviguer vers le dossier scripts
cd c:\Users\Laurent\Pro\Solutyo\recyclage_IT\DB_IT_Product_Management\scripts

# Exécuter les migrations dans l'ordre
psql -U postgres -d ITManagementDB -f migrations\001_initial_schema.sql
psql -U postgres -d ITManagementDB -f migrations\002_add_indexes.sql

# Charger les données de test (optionnel)
psql -U postgres -d ITManagementDB -f seed_data.sql
```

---

### 🧪 3. Vérifier l'installation

Exécutez cette requête pour vérifier que toutes les tables sont créées :

```sql
-- Lister toutes les tables
SELECT 
    table_name,
    table_type
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY table_name;

-- Compter les tables (devrait retourner 15)
SELECT COUNT(*) as nombre_tables
FROM information_schema.tables
WHERE table_schema = 'public' AND table_type = 'BASE TABLE';
```

**Résultat attendu** : 15 tables créées ✅

---

## 📈 Utilisation

### 🔌 Connexion depuis VS Code

**Extension recommandée** : PostgreSQL (par Chris Kolkman)

**Paramètres de connexion** :

- **Host** : `localhost`
- **Port** : `5432`
- **Database** : `ITManagementDB`
- **Username** : `postgres`
- **Password** : [votre mot de passe]

### 📊 Requêtes utiles

| Fichier | Description | Usage |
|---------|-------------|-------|
| `queries/reports.sql` | Rapports standards (produits par statut, performance techniciens) | Reporting quotidien |
| `queries/analytics.sql` | Analyses avancées (taux recyclage, évolution retours) | Analyses stratégiques |

---

## 🏗️ Architecture de la base de données

### 📋 Tables principales

| Table | Description | Rôle |
|-------|-------------|------|
| `returned_products` | **Table centrale** - Produits IT retournés | Tracking principal |
| `product_checkups` | Vérifications techniques effectuées | Suivi qualité |
| `technicians` | Techniciens qualifiés | Gestion équipe |

### 📚 Tables de référence

| Catégorie | Tables | Usage |
|-----------|--------|-------|
| **Acteurs** | `suppliers`, `customers` | Gestion fournisseurs/clients |
| **Configuration** | `os`, `destinations`, `status` | Paramétrage système |
| **Catalogue** | `technical_issues`, `components`, `checkup_tasks` | Référentiels métier |

### 🔗 Tables de jonction (relations N-N)

| Table | Relation | Description |
|-------|----------|-------------|
| `product_issues` | Products ↔ Issues | Problèmes signalés par produit |
| `defective_components` | Products ↔ Components | Composants défectueux identifiés |
| `checkup_component_results` | Checkups ↔ Components | Résultats de vérification par composant |
| `checkup_task_completion` | Checkups ↔ Tasks | Suivi d'exécution des tâches |

### 🔑 Clés et contraintes

- **Clés primaires** : Sur toutes les tables
- **Clés étrangères** : Relations avec `ON DELETE CASCADE` ou `RESTRICT`
- **Contraintes CHECK** : Validation des énumérations (status, types, etc.)
- **Index** : Sur toutes les clés étrangères et colonnes fréquemment recherchées
- **Triggers** : Mise à jour automatique des champs `updated_at`

---

## 📝 Maintenance

### ➕ Ajouter une nouvelle migration

Exemple : 003_add_warranty_tracking.sql

   ```sql

2. **Structure recommandée** :

   ```sql
-- ============================================
-- Migration 003: Ajout du tracking garantie
-- Date: YYYY-MM-DD
-- Description: ...
-- ============================================

\c ITManagementDB;

-- Vos modifications SQL ici

SELECT '✅ Migration 003 terminée!' as message;
3. **Tester** sur un environnement de développement

4. **Documenter** dans ce README

5. **Versionner** avec Git

### 🔄 Rollback d'une migration

Si nécessaire, créez un fichier de rollback correspondant :

```

003_add_warranty_tracking_rollback.sql

## 🎨 Visualisation

### Diagramme Entity-Relationship (ER)

📊 **Fichier** : `assets/DB_modelisation/modelDbReturnedProduct.drawio`

**Ouvrir avec** :
📊 **Fichier** : `assets/DB_modelisation/modelDbReturnedProduct.drawio`

**Ouvrir avec** :

- [Draw.io](https://app.diagrams.net/)
- Extension VS Code : "Draw.io Integration"

Le diagramme montre :

- ✅ Toutes les tables et leurs relations
- ✅ Les cardinalités (1-N, N-N)
- ✅ Les clés primaires et étrangères
- ✅ Les principaux attributs
  
## 🔧 Dépannage

### ❌ Erreur : "relation already exists"

**Solution** : Les tables existent déjà, utilisez `DROP TABLE IF EXISTS` ou vérifiez avec :

```sql
SELECT tablename FROM pg_tables WHERE schemaname = 'public';
```

### ❌ Erreur : "permission denied"

**Solution** : Vérifiez que l'utilisateur `postgres` a les droits :

```sql
GRANT ALL PRIVILEGES ON DATABASE "ITManagementDB" TO postgres;
```

### ❌ Extension PostgreSQL ne se connecte pas

**Solutions** :
**Solutions** :

1. Vérifier que PostgreSQL est démarré :

   ```bash
   Get-Service postgresql*
   ```

2. Tester la connexion via psql :

   ```bash
   psql -U postgres -d ITManagementDB
   ```

3. Recharger VS Code : `Ctrl+Shift+P` → "Reload Window"

---

## 📚 Documentation

### Ressources externes

- 📖 [Documentation PostgreSQL 16](https://www.postgresql.org/docs/16/)
- 🎓 [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- 🔧 [pgAdmin Documentation](https://www.pgadmin.org/docs/)

### Scripts utiles

```sql
-- Voir la taille de la base
SELECT pg_size_pretty(pg_database_size('ITManagementDB'));

-- Voir les tables les plus volumineuses
SELECT 
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Statistiques des tables
SELECT 
    schemaname,
    tablename,
    n_live_tup as row_count,
    n_dead_tup as dead_rows,
    last_vacuum,
    last_autovacuum
FROM pg_stat_user_tables
ORDER BY n_live_tup DESC;
```

---

## 🔐 Sécurité

### Bonnes pratiques appliquées

- ✅ **Pas de mots de passe** en dur dans les scripts
- ✅ **Validation des données** via contraintes CHECK
- ✅ **Clés étrangères** avec actions CASCADE/RESTRICT appropriées
- ✅ **Indexes** sur les colonnes sensibles pour éviter les scans complets
- ✅ **Triggers** pour automatiser la cohérence des données

### Recommandations

- 🔒 Créer un utilisateur applicatif avec droits limités
- 🔒 Ne pas utiliser `postgres` en production
- 🔒 Sauvegarder régulièrement avec `pg_dump`
- 🔒 Activer les logs PostgreSQL pour l'audit

---

## 🚀 Roadmap

### Version actuelle : 1.0

- ✅ Schéma complet des 15 tables
- ✅ Index et triggers optimisés
- ✅ Données de test complètes
- ✅ Requêtes de reporting

### Prochaines versions

- [ ] **v1.1** : Ajout tracking garantie
- [ ] **v1.2** : Historisation des changements de statut
- [ ] **v1.3** : Gestion multi-sites
- [ ] **v2.0** : API REST pour accès externe

---

## 👥 Contributeurs

**Développeur principal** : Laurent  
**Organisation** : Solutyo  
**Date de création** : Octobre 2025  

---

## 📄 Licence

© 2025 Solutyo - Tous droits réservés

Pour toute question ou problème :

- 📧 Email : [votre-email@solutyo.fr]
- 📁 Issues : Créer un ticket dans le repository
- 📖 Wiki : Consultez la documentation étendue
Pour toute question ou problème :
- 📧 Email : [votre-email@solutyo.fr]
- 📁 Issues : Créer un ticket dans le repository
- 📖 Wiki : Consultez la documentation étendue

---

**Dernière mise à jour** : 21 octobre 2025  
**Version** : 1.0.0
