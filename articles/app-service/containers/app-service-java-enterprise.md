---
title: 'Compatibilidad con Java Enterprise en Linux: Azure App Service | Microsoft Docs'
description: Guía del desarrollador para implementar aplicaciones de Java Enterprise mediante WildFly con Azure App Service en Linux.
keywords: servicio de aplicación de Azure, aplicación web, linux, oss, java, wildfly, enterprise, java ee, jee, javaee
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/29/2018
ms.author: routlaw
ms.custom: seodec18
ms.openlocfilehash: 8db65fd9a1f271aea4ceb345f4d9dfbb6b9ff8a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60852233"
---
# <a name="java-enterprise-guide-for-app-service-on-linux"></a>Guía de Java Enterprise para App Service en Linux

> [!NOTE] 
> Java Enterprise Edition en App Service Linux está actualmente en versión preliminar. Esta pila está **no** recomendado para el trabajo de producción. Consulte la [guía para desarrolladores de Java](app-service-linux-java.md) para obtener información sobre nuestras pilas de Java SE y Tomcat.

Azure App Service en Linux permite a los desarrolladores de Java para crear, implementar y escalar aplicaciones empresariales de Java (Java EE) en un servicio totalmente administrado basado en Linux.  El entorno de tiempo de ejecución subyacente de Java Enterprise es el servidor de aplicaciones [WildFly](https://wildfly.org/) de código abierto.

En esta guía se incluyen conceptos clave e instrucciones para los desarrolladores de Java Enterprise que usan App Service para Linux. Si nunca ha implementado aplicaciones de Java con Azure App Service para Linux, debe completar antes el [tutorial de inicio rápido de Java](quickstart-java.md). En la [guía para desarrolladores de Java](app-service-linux-java.md) y en las [Preguntas más frecuentes sobre Azure App Service en Linux](app-service-linux-faq.md) encontrará respuestas a las preguntas sobre App Service para Linux que no son específicas de Java Enterprise.

## <a name="scale-with-app-service"></a>Escalado con App Service 

El servidor de aplicaciones WildFly, que se ejecuta en App Service en Linux, se ejecuta en modo independiente, no en una configuración de dominio. Al escalar horizontalmente el plan de App Service, cada instancia de WildFly se configura como un servidor independiente.

 Escale la aplicación vertical u horizontalmente con [reglas de escalado](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started?toc=%2Fazure%2Fapp-service%2Fcontainers%2Ftoc.json) y [aumentando el número de instancias](https://docs.microsoft.com/azure/app-service/web-sites-scale?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json). 

## <a name="customize-application-server-configuration"></a>Personalización de la configuración del servidor de aplicaciones

Las instancias de la aplicación web no tienen estado, por lo que cada instancia nueva iniciada debe configurarse al inicio para admitir la configuración de Wildfly que necesite la aplicación.
Puede escribir un script de Bash de inicio para llamar a la CLI de WildFly para:

- Configurar orígenes de datos
- Configurar proveedores de mensajería
- Incorporar otros módulos y dependencias a la configuración del servidor WildFly.

  El script se ejecuta cuando WildFly está en funcionamiento, pero antes de que se inicie la aplicación. El script debe usar la [CLI de JBOSS](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) llamada desde `/opt/jboss/wildfly/bin/jboss-cli.sh` para configurar el servidor de aplicaciones con cualquier configuración o cambios necesarios una vez que se inicie el servidor. 

No use el modo interactivo de la CLI para configurar WildFly. En su lugar, puede proporcionar un script de comandos a la CLI de JBoss mediante el comando `--file`, por ejemplo:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Cargue el script de inicio `/home/site/deployments/tools` en la instancia de App Service. Consulte [este documento](/azure/app-service/deploy-configure-credentials#userscope) para obtener instrucciones sobre cómo obtener las credenciales de FTP. 

Establezca el campo **Script de inicio** en Azure Portal en la ubicación del script de shell de inicio, por ejemplo `/home/site/deployments/tools/your-startup-script.sh`.

Proporcione la [configuración de la aplicación](/azure/app-service/web-sites-configure#application-settings) para pasar variables de entorno para su uso en el script. La configuración de la aplicación mantiene las cadenas de conexión y otros secretos necesarios para configurar la aplicación fuera del control de versiones.

## <a name="modules-and-dependencies"></a>Módulos y dependencias

Para instalar los módulos y sus dependencias en la ruta de clase WildFly a través de la CLI de JBoss, deberá crear los siguientes archivos en su propio directorio.  Algunos módulos y dependencias pueden necesitar una configuración adicional, como la nomenclatura de JNDI u otra configuración específica de la API, por lo que esta lista es un conjunto mínimo de lo que tendrá que configurar de una dependencia en la mayoría de los casos.

- Un [descriptor del módulo XML](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Este archivo XML define el nombre, los atributos y las dependencias del módulo. En este [archivo module.xml de ejemplo](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) se definen un módulo de Postgres, su dependencia JDBC del archivo JAR y otras dependencias del módulo necesarias.
- Las dependencias del archivo JAR necesarias para el módulo.
- Un script con los comandos de la CLI de JBoss para configurar el módulo nuevo. Este archivo contendrá los comandos que ejecutará la CLI de JBoss para configurar el servidor para usar la dependencia. Para obtener documentación sobre los comandos para agregar módulos, orígenes de datos y proveedores de mensajería, consulte [este documento](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
-  Un script de inicio de Bash para llamar a la CLI de JBoss y ejecutar el script del paso anterior. Este archivo se ejecutará cuando se reinicie la instancia de App Service o cuando se aprovisionen nuevas instancias durante un escalado horizontal.  Este script de inicio es donde puede realizar otras configuraciones para la aplicación, ya que los comandos de JBoss se pasan a la CLI de JBoss. Como mínimo, este archivo puede ser un único comando para pasar el script de comandos de la CLI de JBoss a la CLI de JBoss: 
   
```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli` 
``` 

Una vez que tenga los archivos y el contenido para el módulo, siga estos pasos para agregar el módulo al servidor de aplicaciones de WildFly. 

1. Cargue en FTP los archivos en `/home/site/deployments/tools` en la instancia de App Service. Consulte este documento para obtener instrucciones sobre cómo obtener las credenciales de FTP. 
2. En la hoja Configuración de la aplicación de Azure Portal, establezca el campo "Script de inicio" en Azure Portal en la ubicación del script de shell de inicio, por ejemplo `/home/site/deployments/tools/your-startup-script.sh`.
3. Para reiniciar la instancia de App Service, presione el botón **Reiniciar** en la sección **Información general** del Portal o mediante la CLI de Azure.

## <a name="data-sources"></a>Orígenes de datos

Para configurar WildFly para una conexión de orígenes de datos, siga el mismo proceso descrito anteriormente en la sección Módulos y dependencias. Puede seguir los mismos pasos para cualquier servicio de Azure Database.

1. Descargue el controlador JDBC para el tipo de base de datos. Para mayor comodidad, estos son los controladores para [Postgres](https://jdbc.postgresql.org/download.html) y [MySQL](https://dev.mysql.com/downloads/connector/j/). Desempaquete la descarga para obtener el archivo .jar.
2. Siga los pasos detallados en "Módulos y dependencias" para crear y cargar el descriptor del módulo XML, el script de la CLI de JBoss, el script de inicio y la dependencia JDBC de .jar.


Hay disponible más información sobre cómo configurar WildFly con [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource) y [SQL Database](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898). Puede usar estas instrucciones personalizadas junto con el enfoque generalizado anterior para agregar definiciones de orígenes de datos al servidor.

## <a name="messaging-providers"></a>Proveedores de mensajería

Para habilitar Beans controlados por mensajes usando Service Bus como mecanismo de mensajería:

1. Use la [biblioteca de mensajería de Apache QPId JMS](https://qpid.apache.org/proton/index.html). Incluya esta dependencia en pom.xml (u otro archivo de compilación) de la aplicación.

2.  Cree [Recursos de Service Bus](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Cree un espacio de nombres de Azure Service Bus, una cola dentro de ese espacio de nombres y una directiva de acceso compartido con funcionalidades de envío y recepción.

3. Pase la clave de la directiva de acceso compartido al código codificando la dirección URL de la clave principal de la directiva o [usando el SDK de Service Bus](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Siga los pasos descritos en la sección Módulos y dependencias con su descriptor XML de módulo, las dependencias de .jar, los comandos de la CLI de JBoss y el script de inicio para el proveedor de JMS. Además de los cuatro archivos, también deberá crear un archivo XML que defina el nombre JNDI de la cola de JMS y el tema. Consulte [este repositorio](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) para obtener los archivos de configuración de referencia.


## <a name="configure-session-management-caching"></a>Configuración del almacenamiento en caché de administración de sesiones

De forma predeterminada, App Service en Linux usará las cookies de afinidad de sesión para asegurarse de que las solicitudes de los clientes con sesiones existentes se enrutan a la misma instancia de la aplicación. Este comportamiento predeterminado no requiere configuración, pero tiene algunas limitaciones:

- Si una instancia de aplicación se reinicia o se reduce verticalmente, se perderá el estado de sesión de usuario en el servidor de aplicaciones.
- Si las aplicaciones tienen una configuración de tiempo de espera de sesión es demasiado largo o un número fijo de usuarios, las nuevas instancias escaladas automáticamente pueden tardar en recibir la carga, puesto que solo las sesiones nuevas se enrutarán a las instancias recién iniciadas.

Puede configurar WildFly para usar un almacén de sesión externo, como [Azure Cache for Redis](/azure/azure-cache-for-redis/). Deberá [deshabilitar la configuración de afinidad de instancia existente de ARR](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) para desactivar el enrutamiento basado en cookies de sesión y permitir que el almacén de sesiones de WildFly configurado funcione sin interferencias.

## <a name="enable-web-sockets"></a>Habilitación de sockets web

De forma predeterminada, los sockets web están habilitados en App Service. Para empezar a usar WebSockets en su aplicación, consulte [este tutorial de inicio rápido](https://github.com/wildfly/quickstart/tree/master/websocket-hello).

## <a name="logs-and-troubleshooting"></a>Registros y solución de problemas

App Service proporciona herramientas para ayudarle a solucionar problemas relacionados con la aplicación.

-   Para activar el registro, haga clic en **Registros de diagnóstico** en el panel de navegación izquierdo. Haga clic en **Sistema de archivos** para establecer el período de retención y la cuota de almacenamiento y guarde los cambios. Puede encontrar estos registros en `/home/LogFiles/`.
-   [Use SSH para conectarse a la instancia de aplicación](app-service-linux-ssh-support.md) para ver los registros para las aplicaciones en ejecución.
-   Consulte los registros de diagnóstico en el panel **Registros de diagnóstico** del portal o mediante el comando de la CLI de Azure: `az webapp log tail --name <your-app-name> --resource-group <your-apps-resource-group>`
