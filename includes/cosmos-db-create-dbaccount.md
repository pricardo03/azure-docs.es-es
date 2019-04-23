---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 5d57d7e18befba175a5a8a825494ce512644b5a2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805136"
---
1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Seleccione **Crear un recurso** > **Bases de datos** > **Azure Cosmos DB**.
   
   ![El panel de las bases de datos de Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. En la página **Creación de una cuenta de Azure Cosmos DB** , especifique la configuración básica de la nueva cuenta de Azure Cosmos. 
 
    |Configuración|Valor|DESCRIPCIÓN |
    |---|---|---|
    |Subscription|Nombre de la suscripción|Seleccione la suscripción de Azure que desea usar para esta cuenta de Azure Cosmos. |
    |Grupo de recursos|Definición de un nombre de grupo de recursos|Seleccione un grupo de recursos o seleccione **Crear nuevo** y escriba un nombre único para el grupo de recursos nuevo. |
    | Nombre de cuenta|Escriba un nombre único.|Escriba un nombre para identificar la cuenta de Azure Cosmos. Dado que *documents.azure.com* se anexa al identificador que se proporciona para crear el identificador URI, debe usar un identificador único.<br><br>El identificador solo puede contener letras minúsculas, números y el carácter de guion (-). Debe tener una longitud de entre 3 y 31 caracteres.|
    | API|Core (SQL)|La API determina el tipo de cuenta que se va a crear. Azure Cosmos DB proporciona cinco API: Core (SQL) y MongoDB para datos de documento, Gremlin para datos de gráfico, Azure Table y Cassandra. Actualmente, debe crear una cuenta independiente para cada API. <br><br>Seleccione **Core(SQL)** para crear una base de datos de documentos y consultarla mediante la sintaxis SQL. <br><br>[Más información acerca de SQL API](../articles/cosmos-db/documentdb-introduction.md).|
    | Ubicación|Seleccionar la región más cercana a los usuarios|Seleccione una ubicación geográfica para hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para que puedan acceder de la forma más rápida posible a los datos.|
   
   ![Página de la nueva cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

1. Seleccione **Revisar + crear**. Puede omitir las secciones **Red** y **Etiquetas**. 

1. Revise la configuración de la cuenta y seleccione **Crear**. La operación de creación de la cuenta tarda unos minutos. Espere hasta que la página del portal muestre **Se completó la implementación** . 

    ![El panel de las notificaciones de Azure Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Seleccione **Ir al recurso** para ir a la página de la cuenta de Azure Cosmos DB. 

    ![La página de la cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
