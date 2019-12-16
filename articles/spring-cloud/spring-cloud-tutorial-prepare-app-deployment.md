---
title: 'Tutorial: Preparación de una aplicación de Spring para su implementación en Azure Spring Cloud'
description: En este tutorial, se prepara una aplicación de Java Spring para su implementación.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: e112fdc9e6f518e2ea3c72161e8978118cf19335
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74890321"
---
# <a name="tutorial-prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Tutorial: Preparación de una aplicación Java Spring para su implementación en Azure Spring Cloud

En este inicio rápido se muestra cómo preparar una aplicación Java Spring Cloud existente para su implementación en Azure Spring Cloud.  Si se ha configurado correctamente, Azure Spring Cloud proporciona servicios sólidos para supervisar, escalar y actualizar cualquier aplicación Spring Cloud. 

## <a name="java-runtime-version"></a>Versión del entorno de ejecución de Java

Las aplicaciones de Spring/Java son las únicas que se pueden ejecutar en Azure Spring Cloud.

Se admiten tanto Java 8 como Java 11. El entorno de hospedaje contiene la versión más reciente de Azul Zulu OpenJDK para Azure. Consulte [este artículo](https://docs.microsoft.com/azure/java/jdk/java-jdk-install) para más información sobre Azul Zulu OpenJDK para Azure. 

## <a name="spring-boot-and-spring-cloud-versions"></a>Versiones de Spring Boot y Spring Cloud

Las aplicaciones de Spring Boot son las únicas que se admiten en Azure Spring Cloud. Se admiten Spring Boot 2.0 y 2.1. En la tabla siguiente se muestran las combinaciones de Spring Boot y Spring Cloud admitidas.

Versión de Spring Boot | Versión de Spring Cloud
---|---
2.0.x | Finchley.RELEASE
2.1.x | Greenwich.RELEASE

Compruebe que el archivo `pom.xml` tiene las dependencias de Spring Boot y Spring Cloud basadas en su versión.

### <a name="version-20"></a>Versión 2.0:

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.9.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Finchley.SR4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="version-21"></a>Versión 2.1:

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.8.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR3</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

## <a name="azure-spring-cloud-client-dependency"></a>Dependencia de cliente de Azure Spring Cloud

Azure Spring Cloud hospeda y administra automáticamente los componentes de Spring Cloud, como Spring Cloud Service Registry y Spring Cloud Config Server. Incluya la biblioteca de cliente de Azure Spring Cloud en sus dependencias para permitir la comunicación con su instancia de servicio de Azure Spring Cloud.

En la tabla siguiente se enumeran las versiones correctas de la aplicación Spring Boot/Spring Cloud.

Versión de Spring Boot | Versión de Spring Cloud | Versión de Azure Spring Cloud
---|---|---
2.0.x | Finchley.RELEASE | 2.0.x
2.1.x | Greenwich.RELEASE | 2.1.x

Incluya uno de los fragmentos de código que aparecen a continuación en su `pom.xml`.  Seleccione el fragmento de código cuya versión coincida con la suya.

### <a name="version-20x"></a>Versión 2.0.x:
```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.0.0</version>
</dependency>
```

### <a name="version-21x"></a>Versión 2.1.x:
```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.0</version>
</dependency>
```

## <a name="other-required-dependencies"></a>Otras dependencias necesarias

Para habilitar las características integradas de Azure Spring Cloud, la aplicación debe incluir las siguientes dependencias. Esto garantiza que la aplicación se configurará automáticamente de forma correcta con cada componente.  

### <a name="service-registry"></a>Registro del servicio

Para usar el servicio Azure Service Registry administrado, incluya `spring-cloud-starter-netflix-eureka-client` en `POM.xml`, como se muestra a continuación.

El punto de conexión del servidor de Service Registry se insertará automáticamente como variables de entorno en la aplicación. Las aplicaciones podrán registrarse en el servidor de Service Registry y detectar otros microservicios dependientes.

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

### <a name="distributed-configuration"></a>Configuración distribuida

Para habilitar la configuración distribuida, incluya `spring-cloud-config-client` en la sección de dependencias de `pom.xml`.

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> No especifique `spring.cloud.config.enabled=false` en la configuración de arranque, ya que impedirá que la aplicación funcione con Config Server.

### <a name="metrics"></a>Métricas

Incluya `spring-boot-starter-actuator` en la sección de dependencias del archivo pom.xml. Las métricas se extraerán periódicamente de los puntos de conexión de JMX y se pueden ver en Azure Portal.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### <a name="distributed-tracing"></a>Seguimiento distribuido

Incluya `spring-cloud-starter-sleuth` y `spring-cloud-starter-zipkin` en la sección de dependencias del archivo pom.xml, como se muestra a continuación. Además, debe habilitar una instancia de Azure App Insights para que funcione con su instancia de servicio de Azure Spring Cloud. [Aquí](spring-cloud-tutorial-distributed-tracing.md) puede encontrar más información sobre cómo habilitar App Insights con Azure Spring Cloud

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a configurar una aplicación Java Spring para su implementación en Azure Spring Cloud.  Para aprender a habilitar Config Server, vaya al siguiente tutorial.

> [!div class="nextstepaction"]
> [Aprenda a configurar Config Server](spring-cloud-tutorial-config-server.md).

Hay más ejemplos disponibles en GitHub: [Ejemplos de Azure Spring Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
