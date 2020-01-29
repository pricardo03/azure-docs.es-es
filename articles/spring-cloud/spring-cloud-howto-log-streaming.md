---
title: Streaming de registros de aplicaciones de Azure Spring Cloud en tiempo real
description: Uso del streaming de registros para ver los registros de aplicaciones al instante
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.openlocfilehash: 27978d367ded7a31d73949cd675ae9e6f8cb887c
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264006"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Streaming de registros de aplicaciones de Azure Spring Cloud en tiempo real
Azure Spring Cloud permite el streaming de registros en la CLI de Azure para obtener registros de la consola de la aplicación en tiempo real para solucionar problemas. También puede realizar el [análisis de registros y métricas con la configuración de diagnóstico](./diagnostic-services.md).

## <a name="prerequisites"></a>Prerequisites

* Instale la [extensión de la CLI de Azure](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) para Spring Cloud, la versión mínima es 0.2.0.
* Una instancia de **Azure Spring Cloud** con una aplicación en ejecución, por ejemplo [Spring Cloud App](./spring-cloud-quickstart-launch-app-cli.md).

## <a name="use-cli-to-tail-logs"></a>Uso de la CLI para el final de los registros

Para evitar especificar repetidamente el nombre del grupo de recursos y el nombre de la instancia de servicio, establezca el nombre del grupo de recursos y el nombre del clúster predeterminados.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
En los siguientes ejemplos, el grupo de recursos y el nombre del servicio se omitirán en los comandos.

### <a name="tail-log-for-app-with-single-instance"></a>Final del registro de la aplicación con una sola instancia
Si una aplicación denominada auth-service solo tiene una instancia, puede ver el registro de la instancia de la aplicación con el siguiente comando:
```
az spring-cloud app log tail -n auth-service
```
Esto devolverá registros:
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Final del registro de una aplicación con varias instancias
Si existen varias instancias de la aplicación denominada `auth-service`, puede ver el registro de la instancia mediante la opción `-i/--instance`. 

En primer lugar, puede obtener los nombres de instancia de la aplicación con el siguiente comando.

```
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
Con resultados:

```
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
Después, puede transmitir los registros de una instancia de la aplicación con la opción `-i/--instance`:

```
az spring-cloud app log tail -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

También puede obtener los detalles de las instancias de la aplicación en Azure Portal.  Después de seleccionar **Aplicaciones** en el panel de navegación izquierdo del servicio Azure Spring Cloud, seleccione **App Instances**.

### <a name="continuously-stream-new-logs"></a>Streaming continuo de nuevos registros
De forma predeterminada, `az spring-cloud ap log tail` imprime solo los registros existentes transmitidos a la consola de la aplicación y, a continuación, se cierra. Si desea hacer streaming de los nuevos registros, agregue-f (--follow):  

```
az spring-cloud app log tail -n auth-service -f
``` 
Para comprobar todas las opciones de registro admitidas:
``` 
az spring-cloud app log tail -h 
```

## <a name="next-steps"></a>Pasos siguientes

* [Análisis de registros y métricas con la configuración de diagnóstico](./diagnostic-services.md)

 





