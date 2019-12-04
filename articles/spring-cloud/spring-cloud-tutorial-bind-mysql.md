---
title: Enlace de una instancia de Azure Database for MySQL con una aplicación de Azure Spring Cloud | Microsoft Docs
description: En este artículo se mostrará cómo enlazar una instancia de Azure Database for MySQL a una aplicación de Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: 6c5cd4ac384affaedbd813f9395f997f92eb69c4
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151126"
---
# <a name="tutorial-bind-an-azure-database-for-mysql-instance-to-your-azure-spring-cloud-application"></a>Tutorial: Enlace de una instancia de Azure Database for MySQL con una aplicación de Azure Spring Cloud 

Con Azure Spring Cloud puede enlazar determinados servicios de Azure con sus aplicaciones automáticamente, en lugar de tener que configurar manualmente la aplicación Spring Boot. En este tutorial se muestra cómo enlazar una aplicación a una instancia de Azure Database for MySQL.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de Azure Spring Cloud implementada
* Una cuenta de Azure Database for MySQL
* CLI de Azure

Si no tiene ninguna instancia de Azure Spring Cloud implementado, siga las instrucciones de [Inicio rápido: Inicio de una aplicación de Azure Spring Cloud desde Azure Portal](spring-cloud-quickstart-launch-app-portal.md) para implementar la primera aplicación de Spring Cloud.

## <a name="bind-your-app-to-your-azure-database-for-mysql-instance"></a>Enlace de una aplicación a una instancia de Azure Database for MySQL

1. Anote el nombre de usuario y la contraseña de administrador de su cuenta de Azure Database for MySQL. 

1. Conéctese al servidor, cree una base de datos llamada **testdb** desde un cliente de MySQL y, después cree una cuenta que no sea de administrador.

1. En el archivo *pom.xml* del proyecto, agregue la siguiente dependencia:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. En el archivo *application.properties*, quite todas las propiedades de `spring.datasource.*`.

1. Actualice la implementación actual mediante la ejecución de `az spring-cloud app update` o cree una implementación para este cambio mediante la ejecución de `az spring-cloud app deployment create`.  Estos comandos actualizan o crean la aplicación con la nueva dependencia.

1. En Azure Portal, en la página del servicio **Azure Spring Cloud**, busque el **Panel de la aplicación** y seleccione la aplicación que va a enlazar a la instancia de Azure Database for MySQL.  Se trata de la misma aplicación que se actualizó o implementó en el paso anterior. 

1. Seleccione **Service binding** (Enlace de servicio) y, después, seleccione el botón **Create service binding** (Crear enlace de servicio). 

1. Rellene el formulario, seleccione **Azure MySQL** en **Tipo de enlace** y use el mismo nombre de base de datos que usó anteriormente, y el nombre de usuario y contraseña que anotó en el primer paso.

1. Tras reiniciar la aplicación el enlace debería funcionar.

1. Para asegurarse de que el enlace del servicio es correcto, seleccione el nombre del enlace y compruebe sus detalles. El campo `property` debería ser similar a este:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a enlazar una aplicación de Azure Spring Cloud a una instancia de Azure Database for MySQL.  Para más información sobre cómo administrar un servicio Azure Spring Cloud, consulte el artículo sobre la detección y registro de servicios.

> [!div class="nextstepaction"]
> [Habilitación de la detección y registro de servicios mediante el registro del servicio de Spring Cloud](spring-cloud-service-registration.md)

