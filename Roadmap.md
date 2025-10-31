# 🇫🇷 Transparence Politique : Facilitons la Démocratie

## 🌟 Vision du Projet

Le projet **Transparence Politique** vise à utiliser nos compétences techniques pour un **devoir civique** : rendre la politique moins opaque et **faciliter la démocratie** dans notre pays.

Notre conviction est qu'un outil simple, libre et gratuit pour trouver de l'information peut rendre le débat/la politique moins opaque.

## 🎯 Objectifs Clés

Notre mission est d'ajouter plus de transparence aux décisions prises à l'Assemblée Nationale.

* **Rendre l'information accessible** à tous, en **vulgarisant** les projets de lois.
* Permettre à chaque citoyen de connaître **les résultats de chacun des votes**.
* Savoir quels votes a soumis le député de sa circonscription, et plus largement, ce que chaque député, groupe ou parti politique vote.

> **Notre But :** S'assurer que le citoyen soit toujours informé du rôle de ses élus.

---

## 🛠️ Documentation Technique Détaillée

### 1. 🥇 Priorisation et Stratégie du MVP (Hackathon)

L'objectif principal du Hackathon est de prouver la capacité à ingérer les données et à les visualiser de manière simple.

| \# | Composant | Action Prioritaire (MVP) | Technologie Clé | Temps Estimé (MVP) |
| :---: | :--- | :--- | :--- | :--- |
| **1** | **Source & ETL** | Récupération, nettoyage et stockage des données de vote et de députés. | **Python + PostgreSQL/Docker** | 40% |
| **2** | **DataViz (Dashboard)** | Affichage simple (liste des votes, stats globales) + Carte interactive. | **Streamlit** (pour la rapidité) ou **Next.js** | 50% |
| **3** | **API Simple** | Point d'accès pour servir les données au dashboard (si Next.js est choisi). | **FastAPI** | 10% |
| **4** | **LLM (RAG)** | *Moyen-terme.* Définir le pipeline, mais ne pas coder la synthèse pour le MVP. | *LangChain + Gemini/Llama 3* | 0% |

---

### 2. 🗄️ Architecture des Données et Stockage

#### 2.1. Ingestion de Données (ETL)

Le processus de collecte et de transformation des données est essentiel.

* **Source de Données :** `https://data.assemblee-nationale.fr/`
    * **Fichiers Clés :** `Scrutins.json.zip` et les données relatives aux députés (disponibles en JSON/XML).
* **Technologie ETL :** **Python** (avec les librairies `requests`, `zipfile`, `pandas`, et `psycopg2` ou `SQLAlchemy` pour PostgreSQL).
* **Infrastructure (MVP) :** Script Python exécuté manuellement ou via un cron job simple.

#### 2.2. Base de Données (BDD)

| Critère | Choix Suggéré | Justification |
| :---: | :--- | :--- |
| **Type** | **PostgreSQL** | Flexibilité (relationnel pour les votes/députés, JSONB pour les données brutes, extension `pgvector` pour l'IA future). |
| **Hébergement** | **Docker** (MVP) / **Google Cloud SQL / AWS RDS** (MVT) | Facile à démarrer localement pour l'équipe / Scalabilité et gestion simplifiée dans le Cloud. |

#### 2.3. Modèle de Données (Schéma de Base SQL)

L'interconnexion de ces trois tables permet de répondre aux objectifs de transparence :

1.  **`deputes`** (Qui ?) : ID, Nom, Groupe Politique, Circonscription.
2.  **`scrutins`** (Quoi ?) : ID Scrutin, Titre de la Loi, Date, Résultat Global (Adopté/Rejeté), Texte intégral de la loi.
3.  **`votes`** (Comment ?) : ID Député (FK), ID Scrutin (FK), Position de Vote (`Pour`, `Contre`, `Abstention`).

```sql
-- Schéma de base de données PostgreSQL pour Transparence Politique

CREATE TABLE deputes (
    id_an VARCHAR(50) PRIMARY KEY, -- ID unique du député de l'Assemblée Nationale
    nom_famille VARCHAR(100) NOT NULL,
    prenom VARCHAR(100),
    sexe VARCHAR(10),
    date_naissance DATE,
    circonscription_num INT,
    circonscription_libelle VARCHAR(255),
    departement VARCHAR(100),
    region VARCHAR(100),
    groupe_politique_id VARCHAR(50), -- ID du groupe politique
    groupe_politique_libelle VARCHAR(255)
);

CREATE TABLE scrutins (
    id_scrutin VARCHAR(50) PRIMARY KEY, -- ID unique du scrutin
    date_scrutin TIMESTAMP WITH TIME ZONE NOT NULL,
    libelle_loi TEXT NOT NULL, -- Intitulé de la loi ou du texte voté
    num_loi VARCHAR(100),
    url_texte_integral TEXT, -- URL vers le texte intégral officiel
    resultat_global VARCHAR(50), -- 'Adopté', 'Rejeté', etc.
    nombre_pour INT,
    nombre_contre INT,
    nombre_abstentions INT,
    nombre_non_votants INT,
    texte_integral TEXT -- Stockage du texte intégral pour l'analyse LLM
);

CREATE TABLE votes (
    id SERIAL PRIMARY KEY, -- Clé primaire auto-incrémentée pour cette table de jointure
    id_depute VARCHAR(50) NOT NULL,
    id_scrutin VARCHAR(50) NOT NULL,
    position_vote VARCHAR(20) NOT NULL, -- 'Pour', 'Contre', 'Abstention', 'Non-votant'
    FOREIGN KEY (id_depute) REFERENCES deputes(id_an),
    FOREIGN KEY (id_scrutin) REFERENCES scrutins(id_scrutin)
);

-- Index pour optimiser les requêtes fréquentes
CREATE INDEX idx_votes_depute ON votes (id_depute);
CREATE INDEX idx_votes_scrutin ON votes (id_scrutin);
CREATE INDEX idx_scrutins_date ON scrutins (date_scrutin);
CREATE INDEX idx_deputes_groupe ON deputes (groupe_politique_libelle);
```

---

### 3. 🖥️ Développement Frontend et DataViz

#### 3.1. Choix du Frontend

* **Option 1 (Vitesse/MVP) :** **Streamlit / Dash (Python)**
    * **Avantages :** Zéro effort de frontend/HTML/CSS requis. Idéal pour une preuve de concept rapide par des data scientists.
    * **Inconvénients :** Moins personnalisable, moins performant pour un grand nombre d'utilisateurs.
* **Option 2 (Qualité/MVT) :** **Next.js (React)**
    * **Avantages :** Performance, personnalisation complète, excellent pour le référencement et l'expérience utilisateur d'un produit commercial.
    * **Inconvénients :** Nécessite plus de temps et de compétences en JavaScript/React.

#### 3.2. Visualisations Clés

* **Dashboard du Député :**
    * Statistiques du vote par alignement (Vs Groupe/Majorité).
    * Liste des lois vulgarisées (résumé court).
* **Carte de la France :**
    * Utiliser **Leaflet** ou **Plotly/Dash** pour afficher la France avec les circonscriptions colorées selon le vote d'un député sur une loi donnée.

---

### 4. 🤖 Architecture LLM/IA (Moyen Terme)

La vulgarisation du contenu législatif nécessite une approche d'IA robuste.

#### 4.1. Approche Anti-Hallucination : RAG

La méthode recommandée est la **Génération Augmentée par Récupération (RAG)** pour baser les synthèses uniquement sur les textes législatifs officiels.

1.  **Pipeline d'indexation :** Texte de Loi Officiel $\rightarrow$ **Vectorisation** (via des modèles d'embeddings) $\rightarrow$ Stockage dans une **Base Vectorielle**.
2.  **Pipeline de Requête :** Requête Utilisateur ("Résume la loi X") $\rightarrow$ Récupération des vecteurs pertinents $\rightarrow$ **LLM (Gemini/Llama)** + Contexte Officiel $\rightarrow$ Résultat fiable.

#### 4.2. Technologies LLM

| Rôle | Technologie Suggérée | Notes |
| :---: | :--- | :--- |
| **Orchestration RAG** | **LangChain** ou **LlamaIndex** | Facilite l'enchaînement des étapes (Récupération, Prompting, Génération). |
| **Base Vectorielle** | **PostgreSQL (avec `pgvector`)** ou **ChromaDB** | Utiliser PostgreSQL est plus simple si vous l'utilisez déjà comme BDD principale. |
| **Modèle de Langage (LLM)** | **Gemini** (via API) ou **Llama 3** (Hébergé) | **Gemini** est rapide et performant pour la synthèse ; Llama 3 est une option open-source robuste. |

---

### 5. 💰 Modèle Économique & Déploiement (Passage au MVT)

#### 5.1. Création d'API (Composant Vendable)

Pour monétiser l'accès aux données transformées (Objectif 2), l'API est essentielle.

* **Technologie :** **FastAPI (Python)**.
    * *Raison :* Extrêmement rapide, offre une documentation automatique (Swagger UI) et est facile à connecter à PostgreSQL.
* **Fonctionnalités :** Endpoints pour filtrer les votes par date/député/parti/loi, et un accès aux résumés ML.

#### 5.2. Stratégie de Déploiement (CI/CD)

Pour une équipe professionnelle (MVT) :

* **Conteneurisation :** Tout conteneuriser avec **Docker** et **Docker Compose**.
* **Pipeline CI/CD :** Utiliser **GitHub Actions** pour un déploiement continu.
* **Plateforme Cloud :** **Google Cloud Platform (GCP)** est un bon choix pour l'intégration de l'IA (Vertex AI, Cloud Run).

---

## 🚀 Démarrage Rapide pour le MVP (Hackathon)

Suivez ces étapes pour lancer rapidement l'environnement de développement :

1.  **Prérequis :**

    * Python 3.9+
    * Docker Desktop installé et en cours d'exécution.
    * Un éditeur de texte (VS Code recommandé).

2.  **Configuration du Projet :**

    ```bash
    # Créez votre dossier de projet
    mkdir transparence-politique
    cd transparence-politique

    # Créez un environnement virtuel Python
    python -m venv venv
    source venv/bin/activate # Sur Windows: .\venv\Scripts\activate

    # Installez les dépendances Python de base
    pip install requests pandas psycopg2-binary
    ```

3.  **Lancement de PostgreSQL avec Docker :**
    Créez un fichier `docker-compose.yml` à la racine de votre projet :

    ```yaml
    version: '3.8'
    services:
      db:
        image: postgres:15-alpine
        restart: always
        environment:
          POSTGRES_DB: transparence_politique
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
        ports:
          - "5432:5432"
        volumes:
          - db_data:/var/lib/postgresql/data

    volumes:
      db_data:
    ```

    Lancez le conteneur :

    ```bash
    docker-compose up -d
    ```

4.  **Création des Tables de la Base de Données :**
    Connectez-vous à la base de données PostgreSQL (vous pouvez utiliser `psql` ou un outil comme DBeaver/pgAdmin) et exécutez le script SQL fourni dans la section `2.3. Modèle de Données (Schéma de Base SQL)`.

5.  **Développement ETL :**
    Créez un script Python (`etl_assemblee_nationale.py`) pour télécharger, extraire et insérer les données de `data.assemblee-nationale.fr` dans votre base de données PostgreSQL.

6.  **Développement du Dashboard (Streamlit - MVP Rapide) :**
    Installez Streamlit : `pip install streamlit`
    Créez un fichier `app.py` et commencez à construire votre tableau de bord en Python.