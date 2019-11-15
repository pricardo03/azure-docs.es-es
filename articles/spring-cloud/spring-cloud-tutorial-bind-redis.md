---
title: Enlace de Azure Cache for Redis con una aplicación de Azure Spring Cloud | Microsoft Docs
description: Aprenda a enlazar Azure Cache for Redis con una aplicación de Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: a901e4194909df85f53799d5937515e42ea87a69
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607575"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-cache-for-redis"></a>Tutorial: Enlace de los servicios de Azure a una aplicación de Azure Spring Cloud: Azure Cache for Redis

Azure Spring Cloud permite enlazar servicios seleccionados de Azure a las aplicaciones automáticamente, en lugar de tener que configurar manualmente la aplicación de Spring Boot. En este artículo se muestra cómo enlazar una aplicación a Azure Cache for Redis.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de Azure Spring Cloud implementada
* Una instancia de servicio de Azure Cache for Redis
* Extensión de Azure Spring Cloud para la CLI de Azure

Si no tiene una instancia implementada de Azure Spring Cloud, siga los pasos descritos en esta [guía de inicio rápido](spring-cloud-quickstart-launch-app-portal.md) para implementar su primera aplicación de Spring Cloud.

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