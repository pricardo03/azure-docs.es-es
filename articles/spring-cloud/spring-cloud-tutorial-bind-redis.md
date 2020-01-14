---
title: 'Tutorial: Enlace de Azure Cache for Redis a una aplicación de Azure Spring Cloud'
description: En este tutorial se muestra cómo enlazar Azure Cache for Redis con una aplicación de Azure Spring Cloud.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: 662d36f8a25f2f0a21d800b7b1a25e94b13908a7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75461494"
---
# <a name="bind-azure-cache-for-redis-to-your-azure-spring-cloud-application"></a>Enlace de Azure Cache for Redis a una aplicación de Azure Spring Cloud 

En lugar de configurar manualmente las aplicaciones de Spring Boot, puede enlazar automáticamente servicios de Azure seleccionados a las aplicaciones mediante Azure Spring Cloud. En este artículo se muestra cómo enlazar su aplicación a Azure Cache for Redis.

## <a name="prerequisites"></a>Prerequisites

* Una instancia de Azure Spring Cloud implementada
* Una instancia de servicio de Azure Cache for Redis
* Extensión de Azure Spring Cloud para la CLI de Azure

Si no tiene una instancia implementada de Azure Spring Cloud, siga los pasos descritos en el [inicio rápido sobre la implementación de una aplicación de Azure Spring Cloud](spring-cloud-quickstart-launch-app-portal.md).

## <a name="bind-azure-cache-for-redis"></a>Enlace de Azure Cache for Redis

1. Agregue la siguiente dependencia al archivo pom.xml del proyecto:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. Quite las propiedades `spring.redis.*` del archivo `application.properties`.

1. Actualice la implementación actual mediante `az spring-cloud app update` o cree una mediante `az spring-cloud app deployment create`.

1. Vaya a la página del servicio Azure Spring Cloud en Azure Portal. Vaya a **Application Dashboard** (Panel de la aplicación) y seleccione la aplicación para enlazar a Azure Cache for Redis. Esta aplicación es la misma que actualizó o implementó en el paso anterior.

1. Seleccione **Service binding** (Enlace de servicio) y seleccione **Create service binding** (Crear enlace de servicio). Rellene el formulario y asegúrese de seleccionar el valor de **Binding type** (Tipo de enlace) de **Azure Cache for Redis**, el servidor de Azure Cache for Redis y la opción de clave **Primary** (Principal).

1. Reinicie la aplicación. El enlace debería funcionar ahora.

1. Para asegurarse de que el enlace de servicios es correcto, seleccione el nombre del enlace y compruebe sus detalles. El campo `property` debería ser similar a este:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a enlazar una aplicación de Azure Spring Cloud a Azure Cache for Redis. Para más información sobre el enlace de servicios a una aplicación, continúe con el tutorial sobre cómo enlazar una aplicación a una instancia de Azure Database for MySQL.

> [!div class="nextstepaction"]
> [Aprender a enlazar a una instancia de Azure Database for MySQL](spring-cloud-tutorial-bind-mysql.md)
