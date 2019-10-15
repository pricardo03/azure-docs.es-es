---
title: Introducción a Azure Spring Cloud
description: Aprenda las características y ventajas de Azure Spring Cloud para implementar y administrar aplicaciones Java Spring en Azure.
author: jpconnock
ms.service: spring-cloud
ms.topic: overview
ms.date: 10/04/2019
ms.author: jeconnoc
ms.openlocfilehash: c58e6b2a0c0de8295df65b44fbdeaeb2d4461948
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166531"
---
# <a name="what-is-azure-spring-cloud"></a>¿Qué es Azure Spring Cloud?

Azure Spring Cloud facilita la implementación de aplicaciones de microservicio basadas en Spring Boot en Azure sin necesidad de cambiar el código.  Azure Spring Cloud permite a los desarrolladores centrarse en su código, ya que administra el ciclo de vida de las aplicaciones de Spring Cloud.  Spring Cloud proporciona administración del ciclo de vida mediante el uso de una supervisión y un diagnóstico completos, administración de la configuración, detección de servicios, integración de CI/CD e implementaciones blue-green, entre otros.

Al formar parte del ecosistema de Azure, Azure Spring Cloud facilita el enlace con otros servicios de Azure, como almacenamiento, bases de datos, supervisión, etc.

Azure Spring Cloud se encuentra actualmente en versión preliminar.  Para acceder a este servicio mientras se encuentre en versión preliminar [rellene este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u
).

Para empezar, complete el inicio rápido de Spring Cloud mediante la [CLI de Azure](spring-cloud-quickstart-launch-app-cli.md), [Azure Portal](spring-cloud-quickstart-launch-app-portal.md) o [ Maven](spring-cloud-quickstart-launch-app-maven.md).

## <a name="application-configuration"></a>Configuración de aplicaciones

### <a name="spring-cloud-config-server"></a>Spring Cloud Config Server

Azure Spring Cloud Config Server ofrece una configuración externalizada de un sistema distribuido con compatibilidad con cliente y servidor.  Config Server proporciona una ubicación central para administrar las propiedades de la aplicación en todos los entornos.  Para más información, visite la [referencia de Spring Cloud Config Server](https://spring.io/projects/spring-cloud-config.md).

### <a name="enable-bluegreen-deployments"></a>Habilitación de implementaciones blue/green

Azure Spring Cloud admite implementaciones blue/green para liberar y actualizar el código en entornos de producción.  Aprovechar este patrón de administración de cambios permite a los desarrolladores implementar características y cambios de código con la seguridad de que se realizará una reserva inmediata cuando sea necesario.  Azure permite a los desarrolladores concentrarse en escribir código, ya que administra varios entornos de producción y facilita la actualización o reversión de los cambios realizados en el código sin interrumpir la aplicación.  Para más información acerca de los entornos de ensayo y las implementaciones blue/green, visite este [artículo de procedimientos](spring-cloud-howto-staging-environment.md).

### <a name="automate-cicd-pipelines"></a>Automatización de canalizaciones de CI/CD

Azure Spring Cloud proporciona integración con Azure DevOps mediante la CLI de Azure.  Azure DevOps permite automatizar la integración e implementación del código en la aplicación Spring.  Para más información, visite [este artículo](spring-cloud-howto-cicd.md).

### <a name="scale-your-application"></a>Escalado de la aplicación

Azure Spring Cloud permite escalar fácilmente los microservicios en el panel de Azure Spring Cloud.  Tanto el número de vCPU como la cantidad de memoria disponible para los microservicios se pueden escalar o reducir verticalmente para ajustarlas a sus necesidades.  El escalado surte efecto en segundos y no requiere cambios en el código ni la reimplementación de los microservicios.  Para más información, complete este [tutorial](spring-cloud-tutorial-scale-manual.md).

## <a name="application-monitoring"></a>Supervisión de aplicaciones

### <a name="monitor-your-application-using-distributed-tracing-and-azure-app-insights"></a>Supervisión de una aplicación mediante el uso del seguimiento distribuido y de Azure App Insights

Las herramientas de seguimiento distribuido de Spring Cloud permiten a los desarrolladores depurar y supervisar las complejas interconexiones entre los microservicios en su aplicación.  Mediante la integración de [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) en [Azure Application Insights](../azure-monitor/insights/insights-overview.md) Azure proporciona una eficaz funcionalidad de seguimiento distribuido directamente desde Azure Portal.  Para más información, complete este [tutorial](spring-cloud-tutorial-distributed-tracing.md).

## <a name="next-steps"></a>Pasos siguientes

- [Inicie la aplicación de Spring Cloud desde la CLI](spring-cloud-quickstart-launch-app-cli.md)
