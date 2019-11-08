---
title: 'Tutorial: Uso del seguimiento distribuido con Azure Spring Cloud | Microsoft Docs'
description: Aprenda a usar el seguimiento distribuido de Spring Cloud mediante Azure Application Insights
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: a9d2100103cdd5858d0d58cf6ef77a6ccac3745f
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607561"
---
# <a name="tutorial-using-distributed-tracing-with-azure-spring-cloud"></a>Tutorial: Uso del seguimiento distribuido con Azure Spring Cloud

Las herramientas de seguimiento distribuido de Spring Cloud permiten depurar y supervisar incidencias complejas. Azure Spring Cloud integra [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) en [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) para proporcionar una eficaz funcionalidad de seguimiento distribuido desde Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Habilitar el seguimiento distribuido en Azure Portal
> * Agregar Spring Cloud Sleuth a una aplicación
> * Ver los mapas de dependencia de las aplicaciones de microservicios
> * Buscar datos de seguimiento con distintos filtros

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

* Un servicio de Azure Spring Cloud aprovisionado y en ejecución.  Complete [este inicio rápido](spring-cloud-quickstart-launch-app-cli.md) para aprovisionar e iniciar un servicio de Azure Spring Cloud.
    
## <a name="add-dependencies"></a>Adición de dependencias

Habilite el remitente de zipkin que va a enviar a la Web, para lo cual debe agregar la siguiente línea al archivo application.properties:

```xml
spring.zipkin.sender.type = web
```

Si ha seguido nuestra [guía para la preparación de una aplicación de Azure Spring Cloud](spring-cloud-tutorial-prepare-app-deployment.md), puede omitir el siguiente paso. En caso contrario, vaya al entorno de desarrollo local y edite el archivo `pom.xml` para incluir la dependencia de Spring Cloud Sleuth:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-sleuth</artifactId>
            <version>${spring-cloud-sleuth.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-sleuth</artifactId>
    </dependency>
</dependencies>
```

* Vuelva a compilar e implementar el servicio Azure Spring Cloud para reflejar estos cambios. 

## <a name="modify-the-sample-rate"></a>Modificación de la frecuencia de muestreo
Puede cambiar la frecuencia con la que se recopilan los datos de telemetría. Para ello, debe modificar la frecuencia de muestreo. Por ejemplo, si desea reducir la frecuencia de muestreo a la mitad, vaya al archivo `application.properties` y cambie la línea siguiente:

```xml
spring.sleuth.sampler.probability=0.5
```

Si ya tiene una aplicación compilada e implementada, puede modificar la frecuencia de muestreo agregando la línea anterior como una variable de entorno en la CLI de Azure o Azure Portal. 

## <a name="enable-application-insights"></a>Habilitación de Application Insights

1. Vaya a la página del servicio Azure Spring Cloud en Azure Portal.
1. En la sección Supervisión, seleccione **Distributed Tracing** (Seguimiento distribuido).
1. Seleccione **Editar configuración** para editar o agregar una configuración nueva.
1. Cree una consulta de Application Insights o seleccione una existente.
1. Elija la categoría de registro que desea supervisar y especifique el tiempo de retención (en días).
1. Seleccione **Aplicar** para aplicar el nuevo seguimiento.

## <a name="view-application-map"></a>Visualización del mapa de aplicación

Vuelva a la página Distributed Tracing (Seguimiento distribuido) y seleccione **View application map** (Ver mapa de aplicación). Revise la representación visual de la configuración de la aplicación y de la supervisión. Para aprender a usar el mapa de aplicación, consulte [este artículo](https://docs.microsoft.com/azure/azure-monitor/app/app-map).

## <a name="search"></a>Search

Utilice la función de búsqueda para consultar otros elementos de telemetría concretos. En la página **Distributed Tracing** (Seguimiento distribuido), seleccione **Buscar**. Para más información sobre cómo usar la función de búsqueda, consulte [este artículo](https://docs.microsoft.com/azure/azure-monitor/app/diagnostic-search).

## <a name="application-insights-page"></a>Página de Application Insights

Application Insights proporciona funcionalidades de supervisión, además del mapa de aplicación y la búsqueda. Busque en Azure Portal el nombre de la aplicación y, después, inicie una página de Application Insights para encontrar más. Para más información sobre cómo usar estas herramientas, [consulte la documentación](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).


## <a name="disable-application-insights"></a>Deshabilitación de Application Insights

1. Vaya a la página del servicio Azure Spring Cloud en Azure Portal.
1. En la sección Supervisión, haga clic en **Distributed Tracing** (Seguimiento distribuido).
1. Haga clic en **Deshabilitar** para deshabilitar Application Insights

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a habilitar y conocer el seguimiento distribuido en Azure Spring Cloud. Para aprender a enlazar una aplicación a una instancia de Azure CosmosDB, pase el siguiente tutorial.

> [!div class="nextstepaction"]
> [Aprenda a enlazar una aplicación a Azure CosmosDB](spring-cloud-tutorial-bind-cosmos.md).
