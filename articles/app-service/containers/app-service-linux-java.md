---
title: Compatibilidad con el lenguaje Java para Azure App Service en Linux | Microsoft Docs
description: Guía del desarrollador para implementar aplicaciones de Java con Azure App Service en Linux.
keywords: azure app service, aplicación web, linux, oss, java
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
ms.openlocfilehash: 8d15aeb92911a26a9a42a0449a24e8c0fee4467b
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497349"
---
# <a name="java-developers-guide-for-app-service-on-linux"></a>Guía para desarrolladores de Java para App Service en Linux

Azure App Service en Linux permite a los desarrolladores de Java compilar, implementar y escalar rápidamente sus aplicaciones web empaquetadas de Tomcat o Java Standard Edition (SE) en un servicio basado en Linux totalmente administrado. Implemente aplicaciones con complementos de Maven desde la línea de comandos o en editores, como IntelliJ, Eclipse o Visual Studio Code.

Esta guía incluye conceptos clave e instrucciones para los desarrolladores de Java que usan App Service para Linux. Si nunca ha usado Azure App Service para Linux, debe leer primero la [Guía de inicio rápido de Java](quickstart-java.md). Encontrará respuestas a las preguntas generales sobre el uso de App Service para Linux que no son específicas del desarrollo de Java en las [Preguntas más frecuentes sobre Azure App Service en Linux](app-service-linux-faq.md).

## <a name="logging-and-debugging-apps"></a>Registro y depuración de aplicaciones

Encontrará informes de rendimiento, visualizaciones de tráfico y comprobaciones de mantenimiento de cada aplicación a través de Azure Portal. Consulte [Introducción a los diagnósticos de Azure App Service](/azure/app-service/app-service-diagnostics) para obtener más información sobre cómo acceder a estas herramientas de diagnóstico y cómo usarlas.

## <a name="application-performance-monitoring"></a>Supervisión de rendimiento de aplicaciones

Consulte [Procedimiento: Herramientas de supervisión de rendimiento de las aplicaciones con aplicaciones Java en Azure App Service en Linux](how-to-java-apm-monitoring.md) para ver instrucciones detalladas sobre cómo configurar New Relic y AppDynamics con aplicaciones Java que se ejecutan en App Service en Linux.

### <a name="ssh-console-access"></a>Acceso a la consola SSH 

Dispone de conectividad SSH al entorno Linux que ejecuta su aplicación. Consulte [Compatibilidad con SSH para Azure App Service en Linux](/azure/app-service/containers/app-service-linux-ssh-support) para obtener instrucciones completas sobre la conexión al sistema Linux mediante el explorador web o el terminal local.

### <a name="streaming-logs"></a>Registros de streaming

Para la depuración y la solución de problemas con rapidez, puede transmitir los registros a la consola mediante la CLI de Azure. Configure la CLI con `az webapp log config` para habilitar el registro:

```azurecli-interactive
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
 --web-server-logging filesystem
```

A continuación, transmita registros a la consola mediante `az webapp log tail`:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```

Para obtener más información, consulte el apartado sobre el [streaming de registros con la CLI de Azure](/azure/app-service/web-sites-enable-diagnostic-log#streaming-with-azure-command-line-interface).

### <a name="app-logging"></a>Registro de aplicaciones

Habilite el [registro de aplicaciones](/azure/app-service/web-sites-enable-diagnostic-log#enablediag) a través de Azure Portal o la [CLI de Azure](/cli/azure/webapp/log#az-webapp-log-config) para configurar App Service para que escriba los flujos de salida y de error de la consola estándar en el sistema de archivos local o en Azure Blob Storage. El registro en el sistema de archivos local de App Service se deshabilita 12 horas después de su configuración. Si necesita una retención más prolongada, configure la aplicación para escribir la salida en un contenedor de Blob Storage.

Si la aplicación usa [Logback](https://logback.qos.ch/) o [Log4j](https://logging.apache.org/log4j) para el seguimiento, puede reenviar estos seguimientos para su revisión en Azure Application Insights mediante las instrucciones de configuración del marco de registro en [Exploración de los registros de seguimiento de Java en Application Insights](/azure/application-insights/app-insights-java-trace-logs). 

## <a name="customization-and-tuning"></a>Personalización y optimización

Azure App Service para Linux admite la optimización y la personalización de serie a través de Azure Portal y de la CLI de Azure. Revise los artículos siguientes para la configuración de aplicaciones web específicas que no son de Java:

- [Configuración de App Service](/azure/app-service/web-sites-configure?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Configuración de un dominio personalizado](/azure/app-service/app-service-web-tutorial-custom-domain?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Habilitación de SSL](/azure/app-service/app-service-web-tutorial-custom-ssl?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Adición de una red CDN](/azure/cdn/cdn-add-to-web-app?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

### <a name="set-java-runtime-options"></a>Definición de las opciones de Java Runtime

Para establecer la memoria asignada u otras opciones de entorno de ejecución de JVM en entornos Tomcat y Java SE, establezca JAVA_OPTS tal como se muestra a continuación como un [valor de la aplicación](/azure/app-service/web-sites-configure#app-settings). App Service Linux pasa esta configuración como una variable de entorno para Java Runtime cuando se inicia.

En Azure Portal, en **Configuración de la aplicación** para la aplicación web, cree un nuevo valor de la aplicación denominado `JAVA_OPTS` que incluya valores de configuración adicionales, como `$JAVA_OPTS -Xms512m -Xmx1204m`.

Para definir la configuración de la aplicación desde el complemento de Maven de Azure App Service Linux, agregue etiquetas setting/value en la sección de complementos de Azure. En el ejemplo siguiente se establecen tamaños de montón de Java mínimo y máximo específicos:

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Xms512m -Xmx1204m</value> 
    </property> 
</appSettings> 
```

Los desarrolladores que ejecutan una sola aplicación con una ranura de implementación en su plan de App Service pueden usar las siguientes opciones:

- Instancias B1 y S1: -Xms1024m -Xmx1024m
- Instancias B2 y S2: -Xms3072m -Xmx3072m
- Instancias B3 y S3: -Xms6144m -Xmx6144m


Cuando optimice la configuración del montón de la aplicación, revise los detalles de su plan de App Service y tenga en cuenta distintas necesidades de aplicaciones y ranuras de implementación para encontrar la asignación óptima de memoria.

### <a name="turn-on-web-sockets"></a>Activación de sockets web

Active la compatibilidad con sockets web en Azure Portal en **Configuración de la aplicación** para la aplicación. Deberá reiniciar la aplicación para que la configuración surta efecto.

Active la compatibilidad con los sockets web mediante la CLI de Azure con el comando siguiente:

```azurecli-interactive
az webapp config set -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} --web-sockets-enabled true 
```

A continuación, reinicie su aplicación:

```azurecli-interactive
az webapp stop -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} 
az webapp start -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}  
```

### <a name="set-default-character-encoding"></a>Definición de la codificación de caracteres predeterminada 

En Azure Portal, en **Configuración de la aplicación** para la aplicación web, cree un nuevo valor de la aplicación denominado `JAVA_OPTS` con el valor `$JAVA_OPTS -Dfile.encoding=UTF-8`.

También puede configurar el valor de la aplicación mediante el complemento de Maven de App Service. Agregue las etiquetas setting y value del valor en la configuración del complemento: 

```xml
<appSettings> 
    <property> 
        <name>JAVA_OPTS</name> 
        <value>$JAVA_OPTS -Dfile.encoding=UTF-8</value> 
    </property> 
</appSettings> 
```

## <a name="secure-applications"></a>Aplicaciones seguras

Las aplicaciones de Java que se ejecutan en App Service para Linux presentan el mismo conjunto de [procedimientos recomendados de seguridad](/azure/security/security-paas-applications-using-app-services) que otras aplicaciones. 

### <a name="authenticate-users"></a>usuarios

Configure la autenticación de la aplicación en Azure Portal con la opción **Autenticación y autorización**. Desde allí, puede habilitar la autenticación con Azure Active Directory o con inicios de sesión en redes sociales como Facebook, Google o GitHub. La configuración de Azure Portal solo funciona al configurar un proveedor de autenticación único.  Para obtener más información, consulte [Configuración de una aplicación de App Service para usar el inicio de sesión de Azure Active Directory](/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) y los artículos relacionados de otros proveedores de identidades.

Si tiene que habilitar varios proveedores de inicio de sesión, siga las instrucciones del artículo sobre la [personalización de la autenticación en App Service](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to).

 Los desarrolladores de Spring Boot pueden usar el [iniciador de Spring Boot para Azure Active Directory](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) para proteger las aplicaciones mediante las anotaciones y las API conocidas de Spring Security.

### <a name="configure-tlsssl"></a>Configuración de TLS/SSL

Siga las instrucciones de [Enlace de un certificado SSL personalizado existente](/azure/app-service/app-service-web-tutorial-custom-ssl) para cargar un certificado SSL existente y enlazarlo al nombre de dominio de la aplicación. De forma predeterminada, la aplicación seguirá permitiendo que las conexiones HTTP sigan los pasos específicos del tutorial para aplicar SSL y TLS.

## <a name="tomcat"></a>Tomcat 

### <a name="connecting-to-data-sources"></a>Conexión a orígenes de datos

>[!NOTE]
> Si su aplicación usa Spring Framework o Spring Boot, puede establecer la información de conexión de base de datos para Spring Data JPA como variables de entorno [en el archivo de propiedades de la aplicación]. A continuación, use la [configuración de la aplicación](/azure/app-service/web-sites-configure#app-settings) para definir estos valores para la aplicación en Azure Portal o la CLI de Azure.

Los fragmentos de código de configuración de ejemplo en esta sección usan la base de datos MySQL. Para más información, consulte los documentos de configuración para [MySQL](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-usagenotes-tomcat.html), [JDBC de SQL Server](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server?view=sql-server-2017) y [PostgreSQL](https://jdbc.postgresql.org/documentation/head/index.html).

Para configurar Tomcat para usar las conexiones administradas a las bases de datos con Java Database Connectivity (JDBC) o Java Persistence API (JPA), personalice primero la variable de entorno CATALINA_OPTS que lee Tomcat durante el inicio. Establezca estos valores a través de un valor de la aplicación en el complemento de Maven de App Service:

```xml
<appSettings> 
    <property> 
        <name>CATALINA_OPTS</name> 
        <value>"$CATALINA_OPTS -Dmysqluser=${mysqluser} -Dmysqlpass=${mysqlpass} -DmysqlURL=${mysqlURL}"</value> 
    </property> 
</appSettings> 
```

O bien, un valor de App Service equivalente de Azure Portal.

A continuación, determine si el origen de datos debe estar disponible solo para una aplicación o para todas las aplicaciones que se ejecutan en el plan de App Service.

Para los orígenes de datos de nivel de aplicación: 

1. Agregue un archivo `context.xml` si no existe para su aplicación web y agréguele el directorio `META-INF` del archivo WAR cuando se compile el proyecto.

2. En este archivo, agregue una entrada de ruta de acceso `Context` para vincular el origen de datos a una dirección JNDI.

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Actualizar el archivo `web.xml` de la aplicación para usar en esta el origen de datos.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

Para los recursos de nivel de servidor compartidos:

1. Copie el contenido de `/usr/local/tomcat/conf` en `/home/tomcat/conf` en su instancia de App Service Linux mediante SSH si no tiene ninguna configuración allí todavía.

2. Agregue el contexto al archivo `server.xml`.

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Actualice el archivo `web.xml` de la aplicación para usar en esta el origen de datos.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

4. Asegúrese de que los archivos del controlador JDBC estén disponibles para el cargador de clases de Tomcat. Para ello, colóquelos en el directorio `/home/tomcat/lib`. Para cargar estos archivos en su instancia de App Service, realice los pasos siguientes:  
    1. Instale la extensión webapp de Azure App Service:

      ```azurecli-interactive
      az extension add –name webapp
      ```

    2. Ejecute el siguiente comando de la CLI para crear un túnel SSH desde el sistema local en App Service:

      ```azurecli-interactive
      az webapp remote-connection create –g [resource group] -n [app name] -p [local port to open]
      ```

    3. Establezca la conexión al puerto de tunelización local con el cliente SFTP y cargue los archivos en la carpeta `/home/tomcat/lib`.

5. Reinicie la aplicación App Service Linux. Tomcat restablecerá `CATALINA_HOME` a `/home/tomcat/conf` y usará las clases y la configuración actualizadas.

## <a name="docker-containers"></a>Contenedores de Docker

Para utilizar el JDK de Zulu compatible con Azure en los contenedores, asegúrese de que extraer y usar las imágenes pregeneradas como se documenta en la [página de descarga de Azul Zulu Enterprise for Azure admitida](https://www.azul.com/downloads/azure-only/zulu/) o usar los ejemplos `Dockerfile` del [repositorio de GitHub de Java de Microsoft](https://github.com/Microsoft/java/tree/master/docker).

## <a name="runtime-availability-and-statement-of-support"></a>Disponibilidad en tiempo de ejecución e instrucción de compatibilidad

App Service para Linux es compatible con dos entornos de ejecución para el hospedaje administrado de aplicaciones web Java:

- El [contenedor de servlets Tomcat](http://tomcat.apache.org/) para ejecutar aplicaciones empaquetadas como archivos WAR (archivo web). Las versiones compatibles son 8.5 y 9.0.
- Java SE Runtime Environment para ejecutar aplicaciones como archivos JAR (archivo Java). La única versión principal compatible es Java 8.

## <a name="java-runtime-statement-of-support"></a>Instrucción de compatibilidad de Java Runtime 

### <a name="jdk-versions-and-maintenance"></a>Mantenimiento y versiones de JDK

El JDK (Java Development Kit) compatible con Azure es [Zulu](https://www.azul.com/downloads/azure-only/zulu/) y se distribuye a través de [Azul Systems](https://www.azul.com/).

Las actualizaciones de versión principal se proporcionarán a través de nuevas opciones de entorno de ejecución nuevas en Azure App Service para Linux. Para actualizar a estas versiones más recientes, los clientes deben configurar su implementación de App Service. Asimismo, son responsables de probar y garantizar que la actualización principal satisface sus necesidades.

Los JDK compatibles se revisarán trimestralmente de manera automática en enero, abril, julio y octubre de cada año.

### <a name="security-updates"></a>Actualizaciones de seguridad

Las revisiones y correcciones de las vulnerabilidades de seguridad principales se publicarán en cuanto estén disponibles a través de Azul Systems. Una vulnerabilidad "principal" se define con una puntuación total de 9.0 o superior en el [CVSS (Common Vulnerability Scoring System), versión 2 del NIST](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Desuso y retirada

Si un entorno compatible de Java Runtime se va a retirar, los desarrolladores de Azure que lo usen recibirán un aviso de desuso al menos seis meses antes de su retirada.

### <a name="local-development"></a>Desarrollo local

Los desarrolladores pueden descargar el JDK de Azul Zulu Enterprise Production Edition para el desarrollo local desde el [sitio de descarga de Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Soporte para el desarrollo

El soporte técnico para el [JDK de Azul Zulu compatible con Azure](https://www.azul.com/downloads/azure-only/zulu/) está disponible al desarrollar para Azure o [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) con un [plan de soporte técnico de Azure cualificado](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Compatibilidad con el tiempo de ejecución

Los desarrolladores pueden [abrir una incidencia](/azure/azure-supportability/how-to-create-azure-support-request) con los JDK de Azul Zulu a través de soporte técnico de Azure si tienen un [plan de soporte técnico completo](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Pasos siguientes

Visite el centro de [Azure para desarrolladores de Java](/java/azure/) para encontrar guías de inicio rápido de Azure, tutoriales y documentación de referencia de Java.
