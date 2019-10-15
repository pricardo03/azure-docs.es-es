---
title: Enlace de Azure Cache for Redis con una aplicación de Azure Spring Cloud | Microsoft Docs
description: Aprenda a enlazar Azure Cache for Redis con una aplicación de Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/06/2019
ms.author: v-vasuke
ms.openlocfilehash: d8fa31207baecc80674fb11b492927800676b8a2
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038244"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-cache-for-redis"></a>Tutorial: Enlace de los servicios de Azure a una aplicación de Azure Spring Cloud: Azure Cache for Redis

Azure Spring Cloud permite enlazar servicios seleccionados de Azure a las aplicaciones automáticamente, en lugar de tener que configurar manualmente la aplicación de Spring Boot. En este artículo se muestra cómo enlazar una aplicación a Azure Cache for Redis.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de Azure Spring Cloud implementada
* Una instancia de servicio de Azure Cache for Redis
* Extensión de Azure Spring Cloud para la CLI de Azure

Si fuera necesario, instale la extensión de Azure Spring Cloud para la CLI de Azure, para lo que debe usar el siguiente comando:

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

>[!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo.  Incluye herramientas comunes de Azure preinstaladas, entre las que se incluyen las versiones más recientes de Git, JDK, Maven y la CLI de Azure. Si ha iniciado sesión en su suscripción de Azure, inicie [Azure Cloud Shell](https://shell.azure.com) desde shell.azure.com.  Para más información acerca de Azure Cloud Shell, [lea la documentación](../cloud-shell/overview.md).

## <a name="bind-azure-cache-for-redis"></a>Enlace de Azure Cache for Redis

1. Agregue la siguiente dependencia al `pom.xml` del proyecto.

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. Quite las propiedades `spring.redis.*`, en caso de que las haya, del archivo `application.properties`.

1. Actualice la implementación actual mediante `az spring-cloud app update` o cree una mediante `az spring-cloud app deployment create`.

1. Vaya a la página del servicio Azure Spring Cloud en Azure Portal. Busque el **panel de la aplicación** y seleccione la aplicación que va a enlazar con Azure Cache for Redis.  Se trata de la misma aplicación que se actualizó o implementó en el paso anterior. Luego, elija `Service binding` y, después, seleccione el botón `Create service binding`. Rellene el formulario, y asegúrese de que selecciona el **tipo de enlace** `Azure Cache for Redis`, el servidor de Redis y la opción de clave principal. 

1. Reinicie la aplicación y el enlace debería funcionar.

1. Para asegurarse de que el enlace de servicios es correcto, seleccione el nombre del enlace y compruebe sus detalles. El campo `property` debería ser similar a este:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a enlazar una aplicación de Azure Spring Cloud a una instancia de Azure Cache for Redis.  Para más información sobre el enlace de servicios a una aplicación, continúe con el tutorial para enlazar una aplicación a una base de datos de MySQL.

> [!div class="nextstepaction"]
> [Aprenda a enlazar un servicio de Azure MySql a su servicio Azure Spring Cloud](spring-cloud-tutorial-bind-mysql.md).