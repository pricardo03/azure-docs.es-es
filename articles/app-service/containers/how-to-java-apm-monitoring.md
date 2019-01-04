---
title: 'Configuración de APM de Java y herramientas de supervisión en Linux: Azure App Service'
description: Aprenda a enviar registros y métricas de sus aplicaciones Java que se ejecutan en App Service Linux a los proveedores de APM NewRelic y App Dynamics.
services: app-service\web
author: rloutlaw
manager: angerobe
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 11/27/2018
ms.author: astay;routlaw
ms.custom: seodec18
ms.openlocfilehash: b730c30d51a9f86adc889a9d6718d003674dbc27
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53260470"
---
# <a name="how-to-application-performance-monitoring-tools-with-java-apps-on-azure-app-service-on-linux"></a>Procedimiento: Herramientas de supervisión del rendimiento de las aplicaciones con aplicaciones Java en Azure App Service en Linux

En este artículo se muestra cómo conectar aplicaciones Java implementadas en Azure App Service en Linux con las plataformas de supervisión de aplicaciones (APM) NewRelic y AppDynamics.

## <a name="configure-new-relic"></a>Configuración de New Relic
1. Cree una cuenta de NewRelic en [NewRelic.com](https://newrelic.com/signup).
2. Descargue el agente de Java desde NewRelic; tendrá un nombre de archivo similar a `newrelic-java-x.x.x.zip`.
3. Copie la clave de licencia, ya que la necesitará más tarde para configurar el agente.
4. [Conéctese mediante SSH a su instancia de App Service](/azure/app-service/containers/app-service-linux-ssh-support) y cree un nuevo directorio `/home/site/wwwroot/apm`. 
5. Cargue los archivos desempaquetados del agente de Java de NewRelic en un directorio en `/home/site/wwwroot/apm`. Los archivos del agente deben estar en `/home/site/wwwroot/apm/newrelic`.
6. Modifique el archivo YAML en `/home/site/wwwroot/apm/newrelic/newrelic.yml` y reemplace el marcador de posición de valor de la licencia por su propia clave de licencia.
7. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.
    - Si la aplicación usa **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Si usa **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Si ya tiene una variable de entorno para `JAVA_OPTS` o `CATALINA_OPTS`, anexe la opción `javaagent` al final del valor actual.

## <a name="configure-appdynamics"></a>Configuración de AppDynamics
1. Cree una cuenta de AppDynamics en [AppDynamics.com](https://www.appdynamics.com/community/register/).
1. Descargue el agente de Java desde el sitio web de AppDynamics; el nombre de archivo será similar a `AppServerAgent-x.x.x.xxxxx.zip`.
1. [Conéctese mediante SSH a su instancia de App Service](/azure/app-service/containers/app-service-linux-ssh-support) y cree un nuevo directorio `/home/site/wwwroot/apm`. 
1. Cargue los archivos del agente de Java en un directorio en `/home/site/wwwroot/apm`. Los archivos del agente deben estar en `/home/site/wwwroot/apm/appdynamics`.
1. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.
    - Si usa **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` donde `<YOUR_SITE_NAME>` es el nombre de su instancia de App Service.
    - Si usa **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<YOUR_SITE_NAME>` donde `<YOUR_SITE_NAME>` es el nombre de su instancia de App Service.
