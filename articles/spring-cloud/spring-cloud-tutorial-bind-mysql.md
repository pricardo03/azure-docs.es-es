---
title: Enlace de Azure Database for MySQL con una aplicación de Azure Spring Cloud | Microsoft Docs
description: En este artículo se muestra cómo enlazar Azure MySQL a una aplicación de Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: b6de5bb3b25c111d1b7775ea9570a4ae2cf45042
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607589"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-database-for-mysql"></a>Tutorial: Enlace de los servicios de Azure a una aplicación de Azure Spring Cloud: Azure Database for MySQL

Azure Spring Cloud permite enlazar servicios seleccionados de Azure a las aplicaciones automáticamente, en lugar de tener que configurar manualmente la aplicación de Spring Boot. En este tutorial se muestra cómo enlazar una aplicación a Azure MySQL.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de Azure Spring Cloud implementada
* Una cuenta de Azure Database for MySQL
* CLI de Azure

Si no tiene una instancia implementada de Azure Spring Cloud, siga los pasos descritos en esta [guía de inicio rápido](spring-cloud-quickstart-launch-app-portal.md) para implementar su primera aplicación de Spring Cloud.

## <a name="bind-azure-database-for-mysql"></a>Enlace de Azure Database for MySQL

1. Anote el nombre de usuario de administrador y la contraseña de su cuenta de Azure MySQL. Conéctese al servidor y cree una base de datos llamada `testdb` desde un cliente MySQL. Cree una cuenta que no sea de administrador.

1. Agregue la siguiente dependencia al `pom.xml` del proyecto.

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. Quite las propiedades `spring.datasource.*`, en caso de que las haya, del archivo `application.properties`.

1. Actualice la implementación actual mediante `az spring-cloud app update` o cree una nueva para este cambio mediante `az spring-cloud app deployment create`.  Estos comandos actualizarán o crearán la aplicación con la nueva dependencia.

1. Vaya a la página del servicio Azure Spring Cloud en Azure Portal. Busque el **panel de la aplicación** y seleccione la aplicación que va a enlazar con Azure MySQL.  Se trata de la misma aplicación que se actualizó o implementó en el paso anterior. Luego, seleccione `Service binding` y, después, seleccione el botón `Create service binding`. Rellene el formulario, y asegúrese de que selecciona **Tipo de enlace** `Azure MySQL`, el mismo nombre de base de datos que usó anteriormente y el mismo nombre de usuario y contraseña que anotó en el primer paso.

1. Reinicie la aplicación y el enlace debería funcionar.

1. Para asegurarse de que el enlace de servicios es correcto, seleccione el nombre del enlace y compruebe sus detalles. El campo `property` debería ser similar a este:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a enlazar una aplicación de Azure Spring Cloud a una base de datos de MySQL.  Para más información sobre cómo administrar un servicio Azure Spring Cloud, lea el artículo sobre la detección y el registro de servicios.

> [!div class="nextstepaction"]
> [Aprenda a habilitar la detección y los registros de servicios desde Spring Cloud Service Registry](spring-cloud-service-registration.md).

