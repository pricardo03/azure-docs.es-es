---
title: Enlace de Azure Cosmos DB con la aplicación Azure Spring Cloud | Microsoft Docs
description: Aprenda a enlazar Azure Cosmos DB con la aplicación Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: 76a2b9f305f041a19b8d7ace8234a804825f6a0e
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607128"
---
# <a name="tutorial-bind-an-azure-cosmos-db-to-your-azure-spring-cloud-application"></a>Tutorial: Enlace de Azure Cosmos DB con la aplicación Azure Spring Cloud

Azure Spring Cloud permite enlazar servicios seleccionados de Azure a las aplicaciones automáticamente, en lugar de tener que configurar manualmente la aplicación de Spring Boot. En este artículo se muestra cómo enlazar la aplicación con Azure Cosmos DB.

Requisitos previos:
* Una instancia de Azure Spring Cloud implementada.  Siga nuestro [inicio rápido](spring-cloud-quickstart-launch-app-cli.md) para comenzar.
* Una cuenta de Azure Cosmos DB con un nivel mínimo de permisos de colaborador.

## <a name="bind-azure-cosmos-db"></a>Enlace de Azure Cosmos DB

Azure Cosmos DB tiene cinco tipos de API diferentes que admiten el enlace:

1. Crea una base de datos de Azure Cosmos DB. [Consulte este artículo](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) para ayuda con la creación de la base de datos. Registre el nombre de la base de datos. La nuestra se llama `testdb`.

1. Agregue una de las dependencias siguientes al archivo `pom.xml` de la aplicación Spring Cloud según el tipo de API.
    
    #### <a name="api-type-core-sql"></a>Tipo de API: Core (SQL)

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
        <version>2.1.6</version>
    </dependency>
    ```
    
    #### <a name="api-type-mongodb"></a>Tipo de API: MongoDB

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>
    ```

    #### <a name="api-type-cassandra"></a>Tipo de API: Cassandra

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-cassandra</artifactId>
    </dependency>
    ```

    #### <a name="api-type-gremlin-graph"></a>Tipo de API: Gremlin (graph)

    ```xml
    <dependency>
        <groupId>com.microsoft.spring.data.gremlin</groupId>
        <artifactId>spring-data-gremlin</artifactId>
        <version>2.1.7</version>
    </dependency>
    ```

    #### <a name="api-type-azure-table"></a>Tipo de API: tabla de Azure

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-storage-spring-boot-starter</artifactId>
        <version>2.0.5</version>
    </dependency>
    ```

1. Actualice la implementación actual mediante `az spring-cloud app update` o cree una nueva para este cambio mediante `az spring-cloud app deployment create`.  Estos comandos actualizarán o crearán la aplicación con la nueva dependencia.

1. Vaya a la página del servicio Azure Spring Cloud en Azure Portal. Se trata de la misma aplicación que se actualizó o implementó en el paso anterior. Busque el **panel de la aplicación** y seleccione la aplicación que se va a enlazar a Cosmos DB. Luego, seleccione `Service binding` y, después, seleccione el botón `Create service binding`. Rellene el formulario; seleccione **Tipo de enlace** `Azure Cosmos DB`, el tipo de API, el nombre de la base de datos y la cuenta de Azure Cosmos DB.

    > [!NOTE]
    > Si usa Cassandra, use un espacio de claves para el nombre de la base de datos.

1. Seleccione el botón **Reiniciar** en la página de la aplicación para reiniciar la aplicación.

1. Para asegurarse de que el servicio esté enlazado correctamente, seleccione el nombre del enlace y compruebe sus detalles. El campo `property` debe ser similar al siguiente:

    ```
    azure.cosmosdb.uri=https:/<some account>.documents.azure.com:443
    azure.cosmosdb.key=abc******
    azure.cosmosdb.database=testdb
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a enlazar la aplicación Azure Spring Cloud a Cosmos DB.  Para información sobre cómo enlazar la aplicación a Azure Redis Cache, continúe con el tutorial siguiente.

> [!div class="nextstepaction"]
> [Enlace de una aplicación Azure Spring Cloud a Azure Redis Cache](spring-cloud-tutorial-bind-redis.md).
