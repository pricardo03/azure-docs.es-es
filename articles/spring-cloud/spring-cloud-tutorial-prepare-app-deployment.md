---
title: 'Tutorial: Preparación de una aplicación Java Spring para su implementación en Azure Spring Cloud'
description: En este tutorial se prepara una aplicación Java Spring para su implementación en Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: af3611e4c4d1f5d8ca52b3ceb80d79dcfd7d2061
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190734"
---
# <a name="prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Preparación de una aplicación Java Spring para su implementación en Azure Spring Cloud

En este inicio rápido se muestra cómo preparar una aplicación Java Spring existente para su implementación en Azure Spring Cloud. Si se ha configurado correctamente, Azure Spring Cloud proporciona servicios sólidos para supervisar, escalar y actualizar cualquier aplicación Java Spring Cloud.

## <a name="java-runtime-version"></a>Versión del entorno de ejecución de Java

Las aplicaciones de Spring/Java son las únicas que se pueden ejecutar en Azure Spring Cloud.

Azure Spring Cloud es compatible con Java 8 y Java 11. El entorno de hospedaje contiene la versión más reciente de Azul Zulu OpenJDK para Azure. Para más información sobre Azul Zulu OpenJDK para Azure, consulte [Instalación del JDK para Azure y Azure Stack](https://docs.microsoft.com/azure/java/jdk/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Versiones de Spring Boot y Spring Cloud

Azure Spring Cloud solo admite aplicaciones Spring Boot (versiones Admite tanto la versión 2.1 como la 2.2 de Spring Boot. En la tabla siguiente se enumeran las combinaciones admitidas de Spring Boot y Spring Cloud:

Versión de Spring Boot | Versión de Spring Cloud
---|---
2.1 | Greenwich.RELEASE
2.2 | Hoxton.RELEASE

Compruebe que el archivo pom.xml tenga las dependencias correctas de Spring Boot y Spring Cloud según la versión de Spring Boot.

### <a name="dependencies-for-spring-boot-version-21"></a>Dependencias de Spring Boot, versión 2.1

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.12.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-22"></a>Dependencias de Spring Boot, versión 2.2

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

## <a name="azure-spring-cloud-client-dependency"></a>Dependencia de cliente de Azure Spring Cloud

Azure Spring Cloud hospeda y administra componentes de Spring Cloud automáticamente. Por ejemplo, componentes como Spring Cloud Service Registry y Spring Cloud Config Server. Incluya la biblioteca cliente de Azure Spring Cloud en sus dependencias para permitir la comunicación con su instancia de servicio de Azure Spring Cloud.

En la tabla siguiente se enumeran las versiones correctas de Azure Spring Cloud para la aplicación que usa Spring Boot y Spring Cloud.

Versión de Spring Boot | Versión de Spring Cloud | Versión de Azure Spring Cloud
---|---|---
2.1 | Greenwich.RELEASE | 2.1
2.2 | Hoxton.RELEASE | 2.2

Incluya una de las dependencias siguientes en el archivo pom.xml. Seleccione la dependencia cuya versión de Azure Spring Cloud coincida con la suya.

### <a name="dependency-for-azure-spring-cloud-version-21"></a>Dependencia de Azure Spring Cloud, versión 2.1

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.1</version>
</dependency>
```

### <a name="dependency-for-azure-spring-cloud-version-22"></a>Dependencia de Azure Spring Cloud, versión 2.2

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.2.0</version>
</dependency>
```

## <a name="other-required-dependencies"></a>Otras dependencias necesarias

Para habilitar las características integradas de Azure Spring Cloud, la aplicación debe incluir las siguientes dependencias. Esta inclusión garantiza que la aplicación se configurará por sí sola correctamente con cada componente.  

### <a name="service-registry-dependency"></a>Dependencia de Service Registry

Para usar el servicio Azure Service Registry administrado, incluya la dependencia `spring-cloud-starter-netflix-eureka-client` en el archivo pom.xml, como se muestra aquí:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

El punto de conexión del servidor de Service Registry se inserta automáticamente como variables de entorno con la aplicación. Así, las aplicaciones se pueden registrar por sí solas en el servidor de Service Registry y detectar otros microservicios dependientes.

### <a name="distributed-configuration-dependency"></a>Dependencia de configuración distribuida

Para habilitar la configuración distribuida, incluya la siguiente dependencia `spring-cloud-config-client` en la sección de dependencias del archivo pom.xml:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> No especifique `spring.cloud.config.enabled=false` en la configuración de arranque. De lo contrario, la aplicación dejará de funcionar con Config Server.

### <a name="metrics-dependency"></a>Dependencia de métricas

Incluya la dependencia `spring-boot-starter-actuator` en la sección de dependencias del archivo pom.xml, como se muestra aquí:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 Se extraen métricas periódicamente de los puntos de conexión JMX. Estas métricas se pueden visualizar mediante Azure Portal.

### <a name="distributed-tracing-dependency"></a>Dependencia de seguimiento distribuido

Incluya las dependencias `spring-cloud-starter-sleuth` y `spring-cloud-starter-zipkin` en la sección de dependencias del archivo pom.xml:

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

 También debe habilitar una instancia de Azure Application Insights para que funcione con su instancia de servicio de Azure Spring Cloud. Lea el [tutorial sobre el seguimiento distribuido](spring-cloud-tutorial-distributed-tracing.md) para aprender a usar Application Insights con Azure Spring Cloud.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a configurar una aplicación Java Spring para su implementación en Azure Spring Cloud. Para aprender a configurar una instancia de Config Server, continúe con el tutorial siguiente.

> [!div class="nextstepaction"]
> [Aprender a configurar una instancia de Config Server](spring-cloud-tutorial-config-server.md)

Hay más ejemplos disponibles en GitHub: [Ejemplos de Azure Spring Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples).
