# E-commerce_analyse
# Projet d'Analyse de Données

## Description du Projet
Ce projet vise à réaliser une analyse approfondie d'un jeu de données contenant des informations sur les commandes, les produits, et les clients. L'objectif est d'explorer les données et d'en tirer des insights significatifs qui peuvent aider à la prise de décisions stratégiques.

## Table des Matières
- [Jeu de données](#jeu-de-données)
- [Technologies Utilisées](#technologies-utilisées)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Requêtes SQL](#requêtes-sql)
- [Visualisation des données](#visualisation-des-données)
- [Contributeurs](#contributeurs)


## Jeu de données
Le jeu de données utilisé pour cette analyse est un fichier xls disponible sur [Kaggle](https://www.kaggle.com/datasets/abiodunonadeji/united-state-superstore-sales). 
Il contient les colonnes suivantes :

**Row ID** 
**Order ID**
**Order Date**
**Ship Date**
**Ship Mode**
**Customer ID**
**Customer Name**
**Segment**
**Country**
**City**
**State**
**Postal Code**
**Region**
**Product ID**
**Category**
**Sub-Category**
**Product Name**
**Sales**
**Quantity**
**Discount**
**Profit**

## Technologies Utilisées
- **Langages de Programmation** : Python, SQL
- **Base de Données** : MariaDB (via XAMPP)
- **Outils d'Analyse** : Jupyter Notebook

## Installation
1. Clonez le dépôt.
2. Accédez au répertoire du projet.
3. Installez les dépendances nécessaires.

## Utilisation
1. Assurez-vous que le serveur MariaDB est en cours d'exécution.
2. Importez le dataset dans votre base de données.
3. Exécutez les requêtes SQL fournies pour explorer et analyser les données.

## Requêtes SQL
### Exploration des Données
```sql
-- Quels sont les cinq produits les plus vendus en termes de quantité ?
SELECT Product_Name,SUM(Quantity) as somme_qtt 
FROM table1
GROUP BY Product_Name
ORDER BY somme_qtt DESC 
LIMIT 5;

-- Quel est le volume des ventes mensuelles pour les deux dernières années ?
SELECT YEAR(Order_Date) AS year, MONTH(Order_Date) AS month, SUM(Sales) AS somme_sales
FROM table1
WHERE YEAR(Order_Date) > (YEAR(CURDATE()) - 2)
GROUP BY YEAR(Order_Date), MONTH(Order_Date)
ORDER BY year, month;

-- Quels sont les cinq clients ayant généré le plus de revenus pour l'entreprise ?
SELECT Customer_Name,SUM(Sales) as somme_vente
FROM table1
GROUP BY Customer_Name
ORDER BY somme_vente DESC
LIMIT 5;

-- Combien de clients ont passé plus de 5 commandes distinctes ?
SELECT Customer_Name,COUNT(DISTINCT Order_ID) as nbr_commandes
FROM table1
GROUP BY Customer_Name
HAVING nbr_commandes>5
ORDER BY nbr_commandes DESC;

-- Quelle est la durée moyenne entre la date de commande et la date d'expédition pour chaque mode d'expédition ?
SELECT Ship_Mode,AVG(DATEDIFF(Ship_Date,Order_Date)) AS diff_jours
FROM table1
GROUP BY Ship_Mode;

-- Quel mode d'expédition est le plus rentable ?
SELECT Ship_Mode, SUM(Profit) as somme_profit
FROM table1
GROUP BY Ship_Mode
ORDER BY somme_profit DESC;

-- Dans quelles régions les délais d'expédition sont-ils les plus longs ?
SELECT Region, AVG(DATEDIFF(Ship_Date, Order_Date)) AS diff_jours
FROM table1
GROUP BY Region
ORDER BY diff_jours DESC;
```

### Analyse des Données
```SQL
-- Quelles régions génèrent le plus de profits ?
SELECT State, Region, SUM(Profit) AS somme_profit
FROM table1
GROUP BY State, Region
ORDER BY somme_profit DESC;

-- Quelles catégories de produits enregistrent les plus grandes pertes (profits négatifs) ?
SELECT Category,SUM(Profit) as somme_profit
FROM table1
WHERE Profit<0
GROUP BY Category
ORDER BY somme_profit ASC;

-- Quelle est la moyenne des profits par trimestre ?
SELECT
    CASE 
        WHEN MONTH(Order_Date) IN (1, 2, 3) THEN 'T1'
        WHEN MONTH(Order_Date) IN (4, 5, 6) THEN 'T2'
        WHEN MONTH(Order_Date) IN (7, 8, 9) THEN 'T3'
        ELSE 'T4'
    END AS Trimestre,
    AVG(Profit) AS moyenne_profit
FROM table1
GROUP BY 
    CASE 
        WHEN MONTH(Order_Date) IN (1, 2, 3) THEN 'T1'
        WHEN MONTH(Order_Date) IN (4, 5, 6) THEN 'T2'
        WHEN MONTH(Order_Date) IN (7, 8, 9) THEN 'T3'
        ELSE 'T4'
    END;

-- Dans quel segment se trouvent les clients les plus rentables ?
SELECT Segment, Customer_Name, SUM(Sales) AS somme_vente
FROM table1
GROUP BY Segment, Customer_Name
ORDER BY somme_vente DESC;

-- Trouver les produits qui ont généré plus de ventes que la moyenne des ventes de tous les produits.
WITH cte AS (SELECT Product_Name, SUM(Sales) AS somme_sales
FROM table1
GROUP BY Product_Name)
SELECT * FROM cte
WHERE somme_sales > (SELECT AVG(Sales) FROM table1)
ORDER BY somme_sales DESC;

-- Trouver les produits dont les ventes pendant la période de promotion (Décembre) sont supérieures aux ventes moyennes des autres mois de l'année.
SELECT Product_Name,SUM(Sales) AS somme_ventes, MONTH(Order_Date) AS mois
FROM table1
WHERE MONTH(Order_Date) IN (12)
GROUP BY Product_Name, MONTH(Order_Date)
HAVING SUM(Sales)>(SELECT AVG(Sales)
FROM table1
WHERE MONTH(Order_Date) NOT IN (12));
```

# Visualisation des données

   - Outils utilisés :
            Python (Matplotlib, Seaborn)
            
   - Objectif de la visualisation :
            Calculer les ventes mensuelles du magasin et identifier le mois ayant le plus de revenus ainsi que celui ayant les revenus les plus faibles.
            Analyser les ventes en fonction des catégories de produits et déterminer laquelle génère le plus de revenus et laquelle en génère le moins.
            Analyser les ventes en fonction des sous-catégories de produits et déterminer laquelle génère le plus de revenus et laquelle en génère le moins.
            Calculer le profit mensuel du magasin et identifier le mois ayant le profit le plus élevé ainsi que celui ayant le profit le plus faible.
            Analyser le profit par catégorie et par sous-catégorie.
            Analyser les ventes et les profits par segments de clients.

   - Images de visualisations:
     ![visualisation_pages-to-jpg-0001](https://github.com/user-attachments/assets/0cac76af-a044-4a93-afba-a0d5964ee57e)
     ![visualisation_pages-to-jpg-0002](https://github.com/user-attachments/assets/a589ad01-3a83-4cfc-a61b-18638d745092)
     ![visualisation_pages-to-jpg-0003](https://github.com/user-attachments/assets/b248ef39-5a35-4ffe-83e3-b84b38d1a078)
     ![visualisation_pages-to-jpg-0004](https://github.com/user-attachments/assets/678a85bf-3803-4087-bb86-8a0318e111cd)

# Remarques

### Ventes mensuelles (Graphique en courbes)
Les mois de novembre et décembre enregistrent les chiffres de ventes les plus élevés, probablement en raison de la période des fêtes de fin d'année. Septembre affiche des ventes modérées, tandis que les mois d'avril, mai, juin et juillet présentent des performances moyennes. Les mois de janvier et février enregistrent les chiffres de ventes les plus faibles, ce qui pourrait être lié à une faible demande en début d'année.

### Conseil d'amélioration
    Identifier les facteurs saisonniers ou promotionnels influençant les pics de novembre et décembre pour répliquer ces stratégies sur d'autres périodes.
    Envisager des campagnes marketing ciblées pour dynamiser les ventes durant les mois de faible activité (janvier et février).

### Répartition des ventes par catégorie de produits (Graphique en secteurs/pie chart)
Les catégories "Technology", "Furniture" et "Office Supplies" se partagent les ventes globales avec respectivement 36,4 % (742 000 $), 32,3 % (836 154 $) et 31,3 % (719 047 $).

### Conseil d'amélioration
    Analyser les performances spécifiques de chaque catégorie pour déterminer les produits les plus performants et les leviers de croissance.
    Évaluer si des catégories moins performantes pourraient être développées ou promues pour équilibrer davantage la répartition des ventes.

### Ventes par sous-catégories (Graphique en barres)
Les sous-catégories "Chairs" et "Phones" dominent les ventes. À l'inverse, les sous-catégories comme "Fasteners", "Labels", "Envelopes" et "Art" affichent les ventes les plus faibles.

### Conseil d'amélioration
    Investir dans les sous-catégories les plus performantes pour consolider leur croissance.
    Revoir l'assortiment, les prix ou les promotions pour les sous-catégories les moins performantes afin d’améliorer leur attractivité.

### Total de profit par mois (Graphique en courbes)
Les mois de décembre, novembre et août génèrent les profits les plus élevés. Le mois de mars affiche un profit modéré, tandis que janvier et février se caractérisent par des profits faibles.

### Conseil d’amélioration
    Corréler les profits avec les coûts opérationnels et les stratégies de prix pour maximiser les bénéfices sur les périodes à forte demande.
    Élaborer des promotions stratégiques en début d’année pour stimuler les ventes et améliorer les profits.

### Profit par catégorie (Graphique en secteurs/pie chart)
Les catégories "Technology" et "Office Supplies" dominent avec respectivement 50,8 % (145 455 $) et 42,8 % (122 491 $) des profits totaux. La catégorie "Furniture" enregistre un profit limité à 18 451 $, représentant seulement 6,4 % du profit global.

### Conseil d’amélioration
    Revoir la stratégie de pricing et les coûts associés à la catégorie "Furniture" pour améliorer sa rentabilité.
    Augmenter les investissements marketing pour "Technology" et "Office Supplies", qui montrent un fort potentiel.

### Profit par sous-catégorie (Graphique en barres)
Les sous-catégories "Copiers", "Phones" et "Accessories" réalisent les profits les plus élevés. "Papers", "Binders" et "Chairs" génèrent des profits moyens, tandis que la sous-catégorie "Tables" affiche un profit négatif.

### Conseil d’amélioration
    Optimiser l’offre pour les sous-catégories à profit moyen afin d’augmenter leur contribution globale.

### Ventes et profit par segment (Graphique en barres groupées)
    Le segment "Consumer" enregistre 1 161 401 $ de ventes et 134 119 $ de profit, représentant le segment le plus performant.
    Le segment "Corporate" génère 706 164 $ de ventes et 91 979 $ de profit.
    Le segment "Home Office" affiche 429 653 $ de ventes et 60 299 $ de profit.

### Conseil d’amélioration
    Développer des stratégies de fidélisation et d'acquisition ciblées pour le segment "Consumer" afin de maintenir et renforcer sa position dominante.
    Identifier des opportunités pour augmenter la rentabilité des segments "Corporate" et "Home Office", tels que des offres adaptées ou des programmes promotionnels spécifiques.
  
        
## Contributeurs
- [Younes ZIAT](https://github.com/younes9888) - Analyste de données
