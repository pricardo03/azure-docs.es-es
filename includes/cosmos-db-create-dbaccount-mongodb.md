---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
ms.custom: include file
ms.openlocfilehash: b1dcf395c55e91a98b237f6e3866c97d74b7a97c
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942740"
---
1. En una nueva ventana, inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el menú de la izquierda, seleccione **Crear un recurso**, luego en **Bases de datos** y, finalmente, en **Azure Cosmos DB**, seleccione **Crear**.
   
   ![Captura de pantalla de Azure Portal en la que se resaltan Más servicios y Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. En la página **Crear una cuenta de Azure Cosmos DB**, especifique la configuración de la nueva cuenta de Azure Cosmos DB. 
 
    Configuración|Value|Descripción
    ---|---|---
    Subscription|Su suscripción|Seleccione la suscripción de Azure que quiere usar para esta cuenta de Azure Cosmos DB. 
    Grupo de recursos|Crear nuevo<br><br>A continuación, escriba el mismo nombre único que se proporcionó en el identificador|Seleccione **Crear nuevo**. A continuación, escriba un nombre nuevo de grupo de recursos para la cuenta. Para simplificar, utilice el mismo nombre del identificador. 
    Nombre de cuenta|Escriba un nombre único.|Escriba un nombre único para identificar la cuenta de Azure Cosmos DB. Dado que *mongo.cosmos.azure.com* se anexa al identificador que se proporciona para crear el identificador URI, debe usar un identificador único.<br><br>El identificador solo debe contener letras minúsculas, números y el carácter de guion (-). Debe tener una longitud de entre 3 y 31 caracteres.
    API|API de Azure Cosmos DB para MongoDB|La API determina el tipo de cuenta que se va a crear. Azure Cosmos DB proporciona cinco API: Core(SQL) para bases de datos de documentos, Gremlin para bases de datos de grafos, API MongoDB de Azure Cosmos DB para bases de datos de documentos, Azure Table y Cassandra. Actualmente, debe crear una cuenta independiente para cada API. <br><br>Seleccione **MongoDB**, ya que en este inicio rápido va a crear una colección que funciona con MongoDB.|
    Location|Seleccionar la región más cercana a los usuarios|Seleccione una ubicación geográfica para hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para proporcionarles el acceso más rápido a los datos.
    Versión|3.6|Seleccione el protocolo de conexión de MongoDB versión 3.6 o la 3.2 si desea obtener compatibilidad con versiones anteriores.

    Seleccione **Revisar y crear**. Puede omitir las secciones **Red** y **Etiquetas**. 

    ![Página de la nueva cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. La cuenta tarda unos minutos en crearse. Espere a que el portal muestre la página **¡Enhorabuena! Su cuenta de Cosmos con compatibilidad de protocolo de conexión con MongoDB está lista**.

    ![El panel de las notificaciones de Azure Portal](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
