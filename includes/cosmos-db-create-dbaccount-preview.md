---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.topic: include
ms.date: 11/24/2018
ms.author: dech
ms.custom: include file
ms.openlocfilehash: 6169821d0e81f0af108dccbfa761f5512e352628
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52853521"
---
1. En una nueva ventana del explorador, inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Haga clic en **Crear un recurso** > **Bases de datos** > **Azure Cosmos DB**.
   
   ![El panel de las bases de datos de Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. En la página **Crear una cuenta de Azure Cosmos DB**, especifique la configuración de la nueva cuenta de Azure Cosmos DB. 
 
    Configuración|Valor|DESCRIPCIÓN
    ---|---|---
    Subscription|*Su suscripción*|Seleccione la suscripción de Azure que quiere usar para esta cuenta de Azure Cosmos DB. 
    Grupo de recursos|Crear nuevo<br><br>*Escriba un nombre único*|Seleccione **Crear nuevo** y, después, escriba un nombre nuevo de grupo de recursos para la cuenta. Para simplificar, puede usar el mismo nombre que el de la cuenta. 
    Nombre de cuenta|*Escriba un nombre único*|Escriba un nombre único para identificar la cuenta de Azure Cosmos DB. Dado que *documents.azure.com* se anexa al identificador que se proporciona para crear el identificador URI, debe usar un identificador único.<br><br>El identificador solo puede contener letras minúsculas, números y el carácter guion (-) y debe tener entre 3 y 50 caracteres.
    API|Core (SQL)|La API determina el tipo de cuenta que se va a crear. Azure Cosmos DB proporciona cinco API: SQL (base de datos de documentos), Gremlin (base de datos de grafos), MongoDB (base de datos de documentos), Table API y Cassandra API. Cada API requiere la creación de una cuenta independiente. <br><br>Seleccione **Core (SQL)** porque en este artículo creará una base de datos de documentos y la consultará mediante la sintaxis SQL. <br><br>[Más información acerca de SQL API](../articles/cosmos-db/documentdb-introduction.md)|
    Ubicación|*Seleccione la región más cercana a los usuarios*|Seleccione una ubicación geográfica para hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para proporcionarles el acceso más rápido a los datos.
    Habilitar redundancia geográfica| Déjelo en blanco | Esto crea una versión replicada de la base de datos en una segunda región (emparejada). Déjelo en blanco.  
    Escrituras de varias regiones| Déjelo en blanco | Esto permite que cada una de las regiones de la base de datos sea una región tanto de lectura como de escritura. Déjelo en blanco.  

    Después, haga clic en **Revisar + crear**. Puede omitir las secciones **Red** y **Etiquetas**. 

    ![Página de la nueva cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-preview.png)

    Revise la información de resumen y haga clic en **Crear**. 

    ![La página de resumen de comprobación de cuenta](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-summary-preview.png)

4. La cuenta tarda unos minutos en crearse. Espere a que el portal muestre el mensaje **Se completó la implementación** y haga clic en **Ir al recurso**.     

    ![La página de la cuenta de almacenamiento creada correctamente](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-complete-preview.png)

5. El portal mostrará ahora **Felicidades. Se ha creado su cuenta de Azure Cosmos DB**.

    ![El panel de las notificaciones de Azure Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

