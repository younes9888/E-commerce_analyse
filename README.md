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
        

        
## Contributeurs
- [Younes ZIAT](https://github.com/younes9888) - Analyste de données
