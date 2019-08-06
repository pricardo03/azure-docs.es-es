---
title: 'Tutorial: Compilación de un modelo de agrupación en clústeres en R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En la segunda parte de esta serie de tutoriales de tres partes, deberá crear un modelo K-Means para realizar una agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/29/2019
ms.openlocfilehash: 9f16ebc5acff7bbccc9de28e2fab0d223c6e244b
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640014"
---
# <a name="tutorial-build-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Creación de un modelo de agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar)

En la segunda parte de esta serie de tres tutoriales creará un modelo de K-Means en R para realizar la agrupación en clústeres. En la siguiente parte de esta serie, implementará este modelo en una base de datos SQL con Azure SQL Database Machine Learning Services (versión preliminar).

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Definir el número de clústeres para un algoritmo K-Means
> * Realizar la agrupación en clústeres
> * Analizar los resultados

En la [primera parte](sql-database-tutorial-clustering-model-prepare-data.md) ha aprendido a preparar los datos de una base de datos de Azure SQL para realizar la agrupación en clústeres.

En la [tercera parte](sql-database-tutorial-clustering-model-deploy.md) aprenderá a crear un procedimiento almacenado en una base de datos de Azure SQL que puede realizar una agrupación en clústeres en R basada en datos nuevos.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* En la segunda parte de este tutorial se da por hecho que ha realizado la [**primera parte** ](sql-database-tutorial-clustering-model-prepare-data.md) y que satisface sus requisitos previos.

## <a name="define-the-number-of-clusters"></a>Definición del número de clústeres

Para agrupar en clústeres los datos del cliente, usará el algoritmo **K-Means** de agrupación en clústeres, una de las formas más sencillas y conocidas de agrupar datos.
Puede leer más información acerca de K-Means en [A complete guide to K-means clustering algorithm](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html) (Guía completa sobre el algoritmo de agrupación en clústeres K-means).

El algoritmo acepta dos entradas: Los propios datos y un número predefinido "*k*" que representa el número de clústeres que desea generar.
La salida son *k* clústeres con los datos de entrada particionados entre ellos.

Para determinar el número de clústeres que debe utilizar el algoritmo, use un trazado de la suma de los cuadrados dentro de los grupos entre el número de clústeres extraídos. El número adecuado de clústeres que se debe usar se encuentra en la curva o "codo" del trazado.

```r
# Determine number of clusters by using a plot of the within groups sum of squares,
# by number of clusters extracted. 
wss <- (nrow(customer_data) - 1) * sum(apply(customer_data, 2, var))
for (i in 2:20)
    wss[i] <- sum(kmeans(customer_data, centers = i)$withinss)
plot(1:20, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
```

![Gráfico de codo](./media/sql-database-tutorial-clustering-model-build/elbow-graph.png)

Según el gráfico, parece que *k = 4* sería un buen valor para probar. Ese valor *k* agrupará los clientes en cuatro clústeres.

## <a name="perform-clustering"></a>Realizar la agrupación en clústeres

En el siguiente script de R, usará la función **rxKmeans**, que es la función K-Means del paquete RevoScaleR.

```r
# Output table to hold the customer group mappings.
# This is a table where the cluster mappings will be saved in the database.
return_cluster = RxSqlServerData(table = "return_cluster", connectionString = connStr);
# Set the seed for the random number generator for predictability
set.seed(10);
# Generate clusters using rxKmeans and output key / cluster to a table in SQL database
# called return_cluster
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters=4,
                   outFile=return_cluster,
                   outColName="cluster",
                   extraVarsToWrite=c("customer"),
                   overwrite=TRUE);

# Read the customer returns cluster table from the database
customer_cluster <- rxDataStep(return_cluster);
```

## <a name="analyze-the-results"></a>Analizar los resultados

Ahora que ha realizado la agrupación en clústeres mediante K-Means, el siguiente paso es analizar el resultado y ver si puede encontrar información procesable.

El objeto **clust** contiene los resultados de la agrupación en clústeres K-Means.

```r
#Look at the clustering details to analyze results
clust
```

```results
Call:
rxKmeans(formula = ~orderRatio + itemsRatio + monetaryRatio + 
    frequency, data = customer_returns, outFile = return_cluster, 
    outColName = "cluster", extraVarsToWrite = c("customer"), 
    overwrite = TRUE, numClusters = 4)
Data: customer_returns
Number of valid observations: 37336
Number of missing observations: 0 
Clustering algorithm:  

K-means clustering with 4 clusters of sizes 31675, 671, 2851, 2139
Cluster means:
   orderRatio   itemsRatio monetaryRatio frequency
1 0.000000000 0.0000000000    0.00000000  0.000000
2 0.007451565 0.0000000000    0.04449653  4.271237
3 1.008067345 0.2707821817    0.49515232  1.031568
4 0.000000000 0.0004675082    0.10858272  1.186068
Within cluster sum of squares by cluster:
         1          2          3          4
    0.0000  1329.0160 18561.3157   363.2188
```

Las medias de los cuatro clústeres se proporcionan mediante las variables definidas en la [primera parte](sql-database-tutorial-clustering-model-prepare-data.md#separate-customers):

* *orderRatio* = proporción de pedidos devueltos (número total de pedidos devueltos parcial o totalmente en comparación con el número total de pedidos)
* *itemsRatio* = proporción de artículos devueltos (número total de artículos devueltos en comparación con el número de artículos comprados)
* *monetaryRatio* = proporción de importe devuelto (importe monetario total de los artículos en comparación con el importe adquirido)
* *frequency* = frecuencia de devolución

La minería de datos mediante K-Means a menudo requiere un análisis en profundidad de los resultados y pasos adicionales para comprender mejor cada clúster, pero puede proporcionar algunos buenos clientes potenciales.
Aquí tiene un par de maneras en las que podría interpretar estos resultados:

* El clúster 1 (el clúster más grande) parece estar constituido por un grupo de clientes que no están activos (todos los valores son cero).
* El clúster 3 parece estar constituido por un grupo que destaca en términos de comportamiento de devolución.

## <a name="clean-up-resources"></a>Limpieza de recursos

***Si no va a continuar con este tutorial***, elimine la base de datos tpcxbb_1gb del servidor de Azure SQL Database.

En Azure Portal, haga lo siguiente:

1. En el menú izquierdo de Azure Portal, seleccione **Todos los recursos** o **Bases de datos SQL**.
1. En el campo **Filtrar por nombre...** , escriba **tpcxbb_1gb** y seleccione su suscripción.
1. Seleccione la base de datos **tpcxbb_1gb**.
1. En la página **Información general**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la segunda parte de esta serie de tutoriales, ha hecho lo siguiente:

* Definir el número de clústeres para un algoritmo K-Means
* Realizar la agrupación en clústeres
* Analizar los resultados

Para implementar el modelo de aprendizaje automático que ha creado, siga la tercera parte de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial: Implementación de un modelo de agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-tutorial-clustering-model-deploy.md)