![??? logo](.media/logo.jpg)

# 🇫🇷 Transparence Politique : Facilitons la Démocratie

## 🌟 Vision du Projet

[cite_start]Le projet **Transparence Politique** vise à utiliser nos compétences techniques pour un **devoir civique** [cite: 5] [cite_start]: rendre la politique moins opaque et **faciliter la démocratie** dans notre pays[cite: 5].

[cite_start]Notre conviction est qu'un outil simple, libre et gratuit pour trouver de l'information peut rendre le débat/la politique moins opaque[cite: 3, 4].

## 🎯 Objectifs Clés (Pitch)

[cite_start]Notre mission est d'ajouter plus de transparence aux décisions prises à l'Assemblée Nationale[cite: 10].

* [cite_start]**Rendre l'information accessible** à tous, en **vulgarisant** les projets de lois[cite: 11, 45].
* [cite_start]Permettre à chaque citoyen de connaître **les résultats de chacun des votes**[cite: 11, 45].
* [cite_start]Savoir quels votes a soumis le député de sa circonscription, et plus largement, ce que chaque député, groupe ou parti politique vote[cite: 12, 46].

> **Notre But :** S'assurer que le citoyen soit toujours informé du rôle de ses élus.

## 🛠️ Composants Techniques & MVP

### 1. Sources de Données (Prérequis)
[cite_start]Le projet nécessite l'accès à une base de données recensant pour l'Assemblée Nationale les votes, les votants et ce qu'ils votent[cite: 16].

* [cite_start]**Piste de Source :** Le site `https://data.assemblee-nationale.fr/`[cite: 18].
* [cite_start]**Infrastructure :** Utiliser des IA pour décrypter les lois et les stocker sur une base de données à part afin de minimiser les requêtes aux IA[cite: 35].

### 2. Ordre de Priorité (MVP pour le Hackathon)

| # | Composant | Description |
| :---: | :--- | :--- |
| **1** | **Site Web / DataViz** | [cite_start]**PRIORITÉ ABSOLUE**[cite: 54, 60]. [cite_start]Un dashboard vulgarisé et accessible[cite: 20]. [cite_start]Inclut la visualisation des députés, les statistiques globales, le résumé des lois votées/à venir [cite: 54][cite_start], la page de l'hémicycle [cite: 62][cite_start], et une carte de la France interactive[cite: 70]. |
| **2** | **Création d'API** | [cite_start]Vendre l'accès à l'API par abonnement[cite: 55]. |
| **3** | **LLM / IA Agentique** | [cite_start]Développer un LLM pour synthétiser les actions d'un élu/groupe, en utilisant un workflow multi-IA pour éviter les hallucinations[cite: 22]. |

## 💰 Pistes de Modèle Économique (Rentabilité)

[cite_start]Le modèle cherche un équilibre entre le projet de bien public [cite: 27] et la viabilité :

* [cite_start]**Financement Public :** Être payé par des fonds publics français (POC)[cite: 25, 100].
* [cite_start]**Abonnement Professionnel :** Vendre une base de données avec abonnement payant pour les médias ou entreprises qui travaillent avec ce type de données[cite: 26, 101].
* [cite_start]**Vente de Résultats ML :** Vendre les résultats de l'apprentissage automatique (Machine Learning)[cite: 31, 105].

> [cite_start]**Note Éthique :** La vente de données sur le positionnement politique est l'une des plus chères, mais aussi la moins éthique à vendre et à traiter, même si c'est légal au niveau RGPD[cite: 27, 28, 101].

## 📈 Stratégie (Traction Gap)

[cite_start]Nous devons nous concentrer sur le franchissement du *Traction Gap* (IPR à MVT)[cite: 124, 137].

* [cite_start]**Priorité Ventes :** Mettre beaucoup d'effort sur l'aspect **vente du produit** (le pitch et l'adoption) car c'est la clé du succès[cite: 157].
* [cite_start]**Recherche :** Faire une **TONNE de recherches** et collecter des données avant de prendre des décisions[cite: 158].
* [cite_start]**Architecture :** Mûrir les quatre architectures clés : **Produit, Revenu, Équipe, et Systèmes**[cite: 154]. [cite_start]Éviter d'investir massivement dans les systèmes au début (utiliser des outils simples)[cite: 156].