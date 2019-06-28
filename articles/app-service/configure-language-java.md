---
title: Configuración de aplicaciones de Java en Windows para Azure App Service | Microsoft Docs
description: Obtenga información sobre cómo configurar aplicaciones de Java para que se ejecuten en las instancias de Windows predeterminadas en Azure App Service.
keywords: azure app service, aplicación web, windows, oss, java
services: app-service
author: jasonfreeberg
manager: jeconnock
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe;cephalin
ms.custom: seodec18
ms.openlocfilehash: 82e8936a888cbc99088ab18423e55dd57a3c2e77
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65604143"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Configuración de una aplicación de Java en Windows para Azure App Service

Azure App Service permite a los desarrolladores de Java compilar, implementar y escalar rápidamente sus aplicaciones web empaquetadas de Tomcat o Java Standard Edition (SE) en un servicio basado en Windows totalmente administrado. Implemente aplicaciones con complementos de Maven desde la línea de comandos o en editores, como IntelliJ, Eclipse o Visual Studio Code.

Esta guía incluye conceptos clave e instrucciones para los desarrolladores de Java que usan App Service. Si nunca ha usado Azure App Service, debe leer primero la [Guía de inicio rápido de Java](app-service-web-get-started-java.md). Encontrará respuestas a las preguntas generales sobre el uso de App Service que no son específicas del desarrollo de Java en las [Preguntas más frecuentes sobre Azure App Service en Windows](faq-configuration-and-management.md).

> [!NOTE]
> ¿No encuentra lo que busca? Consulte las [preguntas más frecuentes sobre software de código abierto de Windows](faq-configuration-and-management.md) o la [Guía de configuración de Java para Linux](containers/configure-language-java.md) para obtener información sobre la implementación y la protección de su aplicación Java.

## <a name="configuring-tomcat"></a>Configuración de Tomcat

Para editar `server.xml` de Tomcat u otros archivos de configuración, en primer lugar anote su versión principal de Tomcat en el portal.

1. Encuentre el directorio principal de Tomcat de su versión ejecutando el comando `env`. Busque la variable de entorno que comienza por `AZURE_TOMCAT` y coincide con la versión principal. Por ejemplo, `AZURE_TOMCAT85_HOME` señala al directorio de Tomcat para Tomcat 8.5.
1. Una vez que haya identificado el directorio principal de Tomcat para su versión, copie el directorio de configuración para `D:\home`. Por ejemplo, si `AZURE_TOMCAT85_HOME` tuviera un valor de `D:\Program Files (x86)\apache-tomcat-8.5.37`, la ruta de acceso completa del directorio de configuración copiado sería `D:\home\tomcat\conf`.

Finalmente, reinicie App Service. Las implementaciones deben ir a `D:\home\site\wwwroot\webapps` igual que antes.

## <a name="java-runtime-statement-of-support"></a>Instrucción de compatibilidad de Java Runtime

### <a name="jdk-versions-and-maintenance"></a>Mantenimiento y versiones de JDK

El JDK (Java Development Kit) compatible con Azure es [Zulu](https://www.azul.com/downloads/azure-only/zulu/) y se distribuye a través de [Azul Systems](https://www.azul.com/).

Las actualizaciones de versión principal se proporcionarán a través de nuevas opciones de entorno de ejecución en Azure App Service para Windows. Para actualizar a estas versiones más recientes, los clientes deben configurar su implementación de App Service. Asimismo, son responsables de probar y garantizar que la actualización principal satisface sus necesidades.

Los JDK compatibles se revisarán trimestralmente de manera automática en enero, abril, julio y octubre de cada año.

### <a name="security-updates"></a>Actualizaciones de seguridad

Las revisiones y correcciones de las vulnerabilidades de seguridad principales se publicarán en cuanto estén disponibles a través de Azul Systems. Una vulnerabilidad "principal" se define con una puntuación total de 9.0 o superior en el [CVSS (Common Vulnerability Scoring System), versión 2 del NIST](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Desuso y retirada

Si un entorno compatible de Java Runtime se va a retirar, los desarrolladores de Azure que lo usen recibirán un aviso de desuso al menos seis meses antes de su retirada.

### <a name="local-development"></a>Desarrollo local

Los desarrolladores pueden descargar el JDK de Azul Zulu Enterprise Production Edition para el desarrollo local desde el [sitio de descarga de Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Soporte para el desarrollo

El soporte técnico para el JDK de [Azul Zulu compatible con Azure](https://www.azul.com/downloads/azure-only/zulu/) está disponible mediante Microsoft al desarrollar para Azure o [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) con un [plan de soporte técnico de Azure cualificado](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Compatibilidad con el tiempo de ejecución

Los desarrolladores pueden [abrir una incidencia](/azure/azure-supportability/how-to-create-azure-support-request) con los JDK de Azul Zulu a través de soporte técnico de Azure si tienen un [plan de soporte técnico completo](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Pasos siguientes

En este tema se proporciona la instrucción de compatibilidad de Java Runtime para Azure App Service en Windows.

- Para más información sobre el hospedaje de aplicaciones web con Azure App Service, consulte [Información general de App Service](overview.md).
- Para obtener información acerca del desarrollo de Java en Azure, visite el [centro para desarrolladores de Azure para Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
