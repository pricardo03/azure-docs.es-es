---
title: 'Configuración de aplicaciones de Java en Linux: Azure App Service | Microsoft Docs'
description: Aprenda a configurar aplicaciones Java que se ejecutan en Azure App Service en Linux.
keywords: azure app service, web app, linux, oss, java, java ee, jee, javaee
services: app-service
author: bmitchell287
manager: barbkess
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/26/2019
ms.author: brendm
ms.custom: seodec18
ms.openlocfilehash: 9625870132d088bf1de6df06f05f0cac41a1e7fa
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74144221"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Configuración de una aplicación de Java en Linux para Azure App Service

Azure App Service en Linux permite a los desarrolladores de Java compilar, implementar y escalar rápidamente sus aplicaciones web empaquetadas de Tomcat o Java Standard Edition (SE) en un servicio basado en Linux totalmente administrado. Implemente aplicaciones con complementos de Maven desde la línea de comandos o en editores, como IntelliJ, Eclipse o Visual Studio Code.

En esta guía se incluyen conceptos clave e instrucciones para los desarrolladores de Java que usan un contenedor Linux integrado en App Service. Si nunca ha usado Azure App Service, siga el [inicio rápido de Java](quickstart-java.md) y el [tutorial de Java con PostgreSQL](tutorial-java-enterprise-postgresql-app.md).

## <a name="deploying-your-app"></a>Implementación de la aplicación

Puede usar el [complemento Maven para Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)para implementar archivos .jar y .war. La implementación con entornos de desarrollo integrado populares también se admite con [Azure Toolkit for IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij) o [Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

De lo contrario, el método de implementación dependerá del tipo de archivo:

- Para implementar archivos .war en Tomcat, utilice el punto de conexión `/api/wardeploy/` para realizar el conjunto de rutinas POST en el archivo. Para obtener más información sobre esta API, consulte [este documento](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Para implementar archivos .jar en las imágenes de Java SE, use el punto de conexión `/api/zipdeploy/` del sitio de Kudu. Para obtener más información sobre la API, consulte [este documento](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

No implemente el archivo .war o .jar con el FTP. La herramienta FTP está diseñada para cargar los scripts de inicio, dependencias u otros archivos en tiempo de ejecución. Tenga en cuenta que no es la opción óptima para realizar la implementación de aplicaciones web.

## <a name="logging-and-debugging-apps"></a>Registro y depuración de aplicaciones

Encontrará informes de rendimiento, visualizaciones de tráfico y comprobaciones de mantenimiento de cada aplicación a través de Azure Portal. Para más información, consulte [Introducción a los diagnósticos de Azure App Service](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>Acceso a la consola SSH

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Transmisión de registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Para más información, consulte [Registros de secuencias en Cloud Shell](../troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>Registro de aplicaciones

Habilite el [registro de aplicaciones](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enable-application-logging-windows) a través de Azure Portal o la [CLI de Azure](/cli/azure/webapp/log#az-webapp-log-config) para configurar App Service para que escriba los flujos de salida y de error de la consola estándar en el sistema de archivos local o en Azure Blob Storage. El registro en el sistema de archivos local de App Service se deshabilita 12 horas después de su configuración. Si necesita una retención más prolongada, configure la aplicación para escribir la salida en un contenedor de Blob Storage. Los registros de aplicación de Java y Tomcat pueden encontrarse en el directorio */home/LogFiles/Application/* .

Si la aplicación usa [Logback](https://logback.qos.ch/) o [Log4j](https://logging.apache.org/log4j) para el seguimiento, puede reenviar estos seguimientos para su revisión en Azure Application Insights mediante las instrucciones de configuración del marco de registro en [Exploración de los registros de seguimiento de Java en Application Insights](/azure/application-insights/app-insights-java-trace-logs).

### <a name="troubleshooting-tools"></a>Herramienta para la solución de problemas

Las imágenes de Java integradas usan el sistema operativo [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html). Use el administrador de paquetes `apk` para instalar todas las herramientas o comandos para la solución de problemas.

### <a name="flight-recorder"></a>Caja negra SQL

Todas las imágenes de Java para en App Service tienen instalado Zulu Flight Recorder, por lo que pueda conectarse fácilmente a la máquina virtual Java e Iniciar grabación de un generador de perfiles o generar un volcado del montón.

#### <a name="timed-recording"></a>Grabación temporizada

Para empezar, use SSH en App Service y ejecute el comando `jcmd` para ver una lista de todos los procesos de Java en ejecución. Además del propio jcmd, debería ver la aplicación de Java que se ejecuta con un número de Id. de proceso (pid).

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

Ejecute el siguiente comando para iniciar una grabación de 30 segundos de la máquina virtual Java. Así se generará el perfil de la máquina virtual Java y se creará un archivo JFR denominado *jfr_example.jfr* en el directorio principal. (reemplace 116 por el pid de la aplicación de Java).

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

Durante el intervalo de 30 segundos puede validar que la grabación se lleva a cabo mediante la ejecución de `jcmd 116 JFR.check`. Esto mostrará todas las grabaciones del proceso de Java dado.

#### <a name="continuous-recording"></a>Grabación continua

Puede usar Zulu Flight Recorder para generar un perfil continuamente de una aplicación Java con un impacto mínimo en el rendimiento del runtime ([origen](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)). Para ello, ejecute el siguiente comando de la CLI de Azure para crear una configuración de aplicación denominada JAVA_OPTS con la configuración necesaria. El contenido de la configuración de la aplicación de JAVA_OPTS se pasa al comando `java` cuando se inicia la aplicación.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Cuando se haya iniciado la grabación, puede volcar los datos de grabación actuales en cualquier momento mediante el comando `JFR.dump`.

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

Para más información, consulte la [referencia del comando Jcmd ](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

### <a name="analyzing-recordings"></a>Análisis de grabaciones

Use [FTPS](../deploy-ftp.md) para descargar el archivo JFR en el equipo local. Para analizar el archivo JFR, descargue e instale [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/). Para obtener instrucciones acerca de Zulu Mission Control, consulte la [documentación de Azul](https://docs.azul.com/zmc/) y [las instrucciones de instalación](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

## <a name="customization-and-tuning"></a>Personalización y optimización

Azure App Service para Linux admite la optimización y la personalización de serie a través de Azure Portal y de la CLI de Azure. Consulte los artículos siguientes para conocer la configuración de aplicaciones web específicas que no son de Java:

- [Configuración de aplicaciones](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)
- [Configuración de un dominio personalizado](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Configuración de enlaces SSL](../configure-ssl-bindings.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Adición de una red CDN](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Configuración del sitio de Kudu](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Definición de las opciones de Java Runtime

Para establecer la memoria asignada u otras opciones de runtime de JVM en entornos de Tomcat y Java SE, cree una [configuración de la aplicación](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) denominada`JAVA_OPTS` con las opciones. App Service Linux pasa esta configuración como una variable de entorno para Java Runtime cuando se inicia.

En Azure Portal, en **Configuración de la aplicación** para la aplicación web, cree un nuevo valor de la aplicación denominado `JAVA_OPTS` que incluya valores de configuración adicionales, como `-Xms512m -Xmx1204m`.

Para definir la configuración de la aplicación desde el complemento MavenLinux, agregue las etiquetas setting/value a la sección de complementos de Azure. En el ejemplo siguiente se establece un tamaño del montón de Java mínimo y máximo específico:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Los desarrolladores que ejecutan una sola aplicación con una ranura de implementación en su plan de App Service pueden usar las siguientes opciones:

- Instancias de S1 y B1: `-Xms1024m -Xmx1024m`
- Instancias de S2 y B2: `-Xms3072m -Xmx3072m`
- Instancias de S3 y B3: `-Xms6144m -Xmx6144m`

Cuando optimice la configuración del montón de la aplicación, revise los detalles de su plan de App Service y tenga en cuenta distintas necesidades de aplicaciones y ranuras de implementación para encontrar la asignación óptima de memoria.

Si va a implementar una aplicación de JAR, debería llamarse *app.jar*, con el fin de que la imagen integrada pueda identificar correctamente la aplicación. (el complemento Maven realiza automáticamente este cambio de nombre). Si no desea cambiar el nombre de archivo JAR para *app.jar*, puede cargar un script de shell con el comando para ejecutar el archivo JAR. A continuación, pegue la ruta de acceso completa a este script en el cuadro de texto[Archivo de inicio](app-service-linux-faq.md#built-in-images) de la sección Configuración del portal. El script de inicio no se ejecuta desde el directorio en el que se encuentra. Por lo tanto, use siempre rutas de acceso absolutas para hacer referencia a los archivos del script de inicio (por ejemplo: `java -jar /home/myapp/myapp.jar`).

### <a name="turn-on-web-sockets"></a>Activación de sockets web

Active la compatibilidad con sockets web en Azure Portal en **Configuración de la aplicación** para la aplicación. Deberá reiniciar la aplicación para que la configuración surta efecto.

Active la compatibilidad con los sockets web mediante la CLI de Azure con el comando siguiente:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

A continuación, reinicie su aplicación:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Definición de la codificación de caracteres predeterminada

En Azure Portal, en **Configuración de la aplicación** para la aplicación web, cree un nuevo valor de la aplicación denominado `JAVA_OPTS` con el valor `-Dfile.encoding=UTF-8`.

También puede configurar el valor de la aplicación mediante el complemento de Maven de App Service. Agregue las etiquetas setting y value del valor en la configuración del complemento:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Ajuste del tiempo de expiración del inicio

Si la aplicación de Java es especialmente grande, debe aumentar el límite de tiempo del inicio. Para ello, cree una configuración de aplicación, `WEBSITES_CONTAINER_START_TIME_LIMIT` y establézcalo como el número de segundos que App Service debe esperar antes de agotar el tiempo. El valor máximo es `1800` segundos.

### <a name="pre-compile-jsp-files"></a>Precompilación de archivos JSP

Para mejorar el rendimiento de las aplicaciones Tomcat, puede compilar los archivos JSP antes de realizar la implementación en App Service. Puede usar el [complemento Maven](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) proporcionado por Apache Sling o este [archivo de compilación Ant](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation).

## <a name="secure-applications"></a>Aplicaciones seguras

Las aplicaciones de Java que se ejecutan en App Service para Linux presentan el mismo conjunto de [procedimientos recomendados de seguridad](/azure/security/security-paas-applications-using-app-services) que otras aplicaciones.

### <a name="authenticate-users-easy-auth"></a>Autenticación de usuarios (autenticación sencilla)

Configure la autenticación de la aplicación en Azure Portal con la opción **Autenticación y autorización**. Desde allí, puede habilitar la autenticación con Azure Active Directory o con inicios de sesión en redes sociales como Facebook, Google o GitHub. La configuración de Azure Portal solo funciona al configurar un proveedor de autenticación único. Para obtener más información, consulte [Configuración de una aplicación de App Service para usar el inicio de sesión de Azure Active Directory](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) y los artículos relacionados de otros proveedores de identidades. Si tiene que habilitar varios proveedores de inicio de sesión, siga las instrucciones del artículo sobre la [personalización de la autenticación en App Service](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

#### <a name="tomcat-and-wildfly"></a>Tomcat y Wildfly

La aplicación Tomcat o Wildfly puede acceder a las reclamaciones del usuario directamente desde el servlet de mediante la conversión de la entidad de seguridad de objeto a un objeto del mapa. Este último objeto asignará ca tipo de reclamación a una colección de las notificaciones de dicho tipo. En el código siguiente, `request` es una instancia de `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Ahora puede inspeccionar el objeto `Map` de cualquier notificación específica. Por ejemplo, el fragmento de código siguiente recorre en iteración todos los tipos de notificación e imprime el contenido de cada colección.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Para cerrar la sesión de los usuarios, utilice la ruta de acceso `/.auth/ext/logout`. Para realizar otras acciones, consulte la documentación acerca del [uso de la autenticación y de la autorización de App Service](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to). También hay documentación oficial acerca de la [interfaz HttpServletRequest](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) y sus métodos. Los siguientes métodos de servlet también se hidrata métodos según la configuración de App Service:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Para deshabilitar esta característica, cree una configuración de aplicación denominada `WEBSITE_AUTH_SKIP_PRINCIPAL` con un valor de `1`. Para deshabilitar todos los filtros de servlet que ha agregado App Service, cree una configuración denominada `WEBSITE_SKIP_FILTERS` con un valor de `1`.

#### <a name="spring-boot"></a>Spring Boot

Los desarrolladores de Spring Boot pueden usar el [iniciador de Spring Boot para Azure Active Directory](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) para proteger las aplicaciones mediante las anotaciones y las API conocidas de Spring Security. Asegúrese de aumentar el tamaño máximo del encabezado en el archivo *application.properties*. Se recomienda un valor de `16384`.

### <a name="configure-tlsssl"></a>Configuración de TLS/SSL

Siga las instrucciones de [Protección de un nombre DNS personalizado con un enlace SSL en Azure App Service](../configure-ssl-bindings.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para cargar un certificado SSL y enlazarlo al nombre de dominio de la aplicación. De forma predeterminada, la aplicación seguirá permitiendo que las conexiones HTTP sigan los pasos específicos del tutorial para aplicar SSL y TLS.

### <a name="use-keyvault-references"></a>Uso de referencias de KeyVault

[Azure KeyVault](../../key-vault/key-vault-overview.md) proporciona administración de secretos centralizada con directivas de acceso un historial de directivas. En KeyVault puede almacenar secretos (como contraseñas o cadenas de conexión) y acceder a ellos en la aplicación través de las variables de entorno.

En primer lugar, siga las instrucciones para [conceder a su aplicación acceso a Key Vault](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) y [hace una referencia de KeyVault a su secreto en una configuración de aplicación](../app-service-key-vault-references.md#reference-syntax). Para validar que la referencia se resuelve en el secreto, imprima la variable de entorno mie3ntras acceder de forma remota al terminal de App Service.

Para insertar estos secretos en el archivo de configuración de Spring o Tomcat, use la sintaxis de inserción de variables de entorno (`${MY_ENV_VAR}`). En el caso de los archivos de configuración de Spring, consulte esta documentación acerca de [configuraciones externalizadas](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

### <a name="using-the-java-key-store"></a>Uso del almacén de claves de Java

De forma predeterminada, los certificados públicos o privados [cargados en App Service Linux](../configure-ssl-certificate.md) se cargarán en el almacén de claves de Java cuando se inicie el contenedor. Esto significa que los certificados cargados estarán disponibles en el contexto de conexión al realizar conexiones TLS salientes. Después de cargar el certificado, deberá reiniciar la instancia de App Service para que se cargue en el almacén de claves de Java.

Puede depurar o interactuar con la herramienta de claves de Java si [abre una conexión SSH](app-service-linux-ssh-support.md) a la instancia de App Service y ejecuta el comando `keytool`. Consulte la [documentación de la herramienta de claves](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) para obtener una lista de comandos. Los certificados se almacenan en la ubicación predeterminada de archivos del almacén de claves de Java, `$JAVA_HOME/jre/lib/security/cacerts`.

Puede ser necesaria una configuración adicional para el cifrado de la conexión de JDBC. Consulte la documentación del controlador JDBC elegido.

- [PostgreSQL](https://jdbc.postgresql.org/documentation/head/ssl-client.html)
- [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/connecting-with-ssl-encryption?view=sql-server-ver15)
- [MySQL](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-using-ssl.html)

#### <a name="manually-initialize-and-load-the-key-store"></a>Inicialización y carga manual del almacén de claves

Puede inicializar el almacén de claves y agregar certificados manualmente. Cree una configuración de aplicación, `SKIP_JAVA_KEYSTORE_LOAD`, con un valor de `1` para deshabilitar en App Service la carga automática de los certificados en el almacén de claves. Todos los certificados públicos cargados en App Service a través de Azure Portal se almacenan en `/var/ssl/certs/`. Los certificados privados se almacenan en `/var/ssl/private/`.

Para más información sobre la API KeyStore, consulte [la documentación oficial](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html).

## <a name="configure-apm-platforms"></a>Configuración de plataformas APM

En esta sección se muestra cómo conectar aplicaciones Java implementadas en Azure App Service en Linux con las plataformas de supervisión de aplicaciones (APM) NewRelic y AppDynamics.

### <a name="configure-new-relic"></a>Configuración de New Relic

1. Cree una cuenta de NewRelic en [NewRelic.com](https://newrelic.com/signup).
2. Descargue el agente de Java desde NewRelic; tendrá un nombre de archivo similar a *newrelic-java-x.x.x.zip*.
3. Copie la clave de licencia, ya que la necesitará más tarde para configurar el agente.
4. [Conéctese mediante SSH a su instancia de App Service](app-service-linux-ssh-support.md) y cree un nuevo directorio */home/site/wwwroot/apm*.
5. Cargue los archivos desempaquetados del agente de Java de NewRelic en un directorio de */home/site/wwwroot/apm*. Los archivos del agente deben estar en */home/site/wwwroot/apm/newrelic*.
6. Modifique el archivo YAML en */home/site/wwwroot/apm/newrelic/newrelic.yml* y reemplace el marcador de posición de valor de la licencia por su propia clave de licencia.
7. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.
    - Si la aplicación usa **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Si usa **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Si usa **WildFly**, consulte la documentación de New Relic [aquí](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) para obtener instrucciones acerca de cómo instalar el agente de Java y la configuración de JBoss.

### <a name="configure-appdynamics"></a>Configuración de AppDynamics

1. Cree una cuenta de AppDynamics en [AppDynamics.com](https://www.appdynamics.com/community/register/).
2. Descargue el agente de Java desde el sitio web de AppDynamics; el nombre de archivo será similar a *AppServerAgent-x.x.x.xxxxx.zip*
3. [Conéctese mediante SSH a su instancia de App Service](app-service-linux-ssh-support.md) y cree un nuevo directorio */home/site/wwwroot/apm*.
4. Cargue los archivos del agente de Java en un directorio de */home/site/wwwroot/apm*. Los archivos del agente deben estar en */home/site/wwwroot/apm/appdynamics*.
5. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.
    - Si usa **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` donde `<app-name>` es el nombre de su instancia de App Service.
    - Si usa **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` donde `<app-name>` es el nombre de su instancia de App Service.
    - Si usa **WildFly**, consulte la documentación de AppDynamics [aquí](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) para obtener instrucciones acerca de cómo instalar el agente de Java y la configuración de JBoss.

>  Si ya tiene una variable de entorno para `JAVA_OPTS` o `CATALINA_OPTS`, anexe la opción `-javaagent:/...` al final del valor actual.

## <a name="configure-jar-applications"></a>Configuración de aplicaciones JAR

### <a name="starting-jar-apps"></a>Inicio de aplicaciones JAR

De forma predeterminada, App Service espera que su aplicación JAR se denomine *app.jar*. Si tiene este nombre, se ejecutará automáticamente. En el caso de los usuarios de Maven, puede establecer el nombre del archivo JAR incluyendo `<finalName>app</finalName>` en la sección `<build>` de *pom.xml*. [Puede hacer lo mismo en Gradle](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) estableciendo la propiedad`archiveFileName`.

Si desea usar otro nombre para el archivo JAR, también debe proporcionar el [comando de inicio](app-service-linux-faq.md#built-in-images) que ejecuta el archivo JAR. Por ejemplo, `java -jar my-jar-app.jar`. Puede establecer el valor del comando de inicio en el Portal, en Configuración > Configuración general, o bien con una configuración de aplicación denominada `STARTUP_COMMAND`.

### <a name="server-port"></a>Puerto del servidor

App Service en Linux enruta las solicitudes entrantes al puerto 80, por lo que la aplicación también debe escuchar el puerto 80. Esto se puede hacer en la configuración de la aplicación (como el archivo *application.properties* de Spring) o en el comando de inicio (por ejemplo, `java -jar spring-app.jar --server.port=80`). Consulte la siguiente documentación para los marcos comunes de Java:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play Framework](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Orígenes de datos

### <a name="tomcat"></a>Tomcat

Estas instrucciones se aplican a todas las conexiones de base de datos. Deberá rellenar los marcadores de posición con el nombre de clase de controlador de la base de datos elegido y con el archivo JAR. Se proporciona una tabla con los nombres de clase y las descargas de controladores de las bases de datos más habituales.

| Base de datos   | Nombre de clase de controlador                             | Controlador JDBC                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Descargar](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Descargar](https://dev.mysql.com/downloads/connector/j/) (seleccione "Platform Independent") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Descargar](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Para configurar Tomcat para que use Java Database Connectivity (JDBC) o Java Persistence API (JPA), personalice primero la variable de entorno `CATALINA_OPTS` que lee Tomcat al iniciarse. Establezca estos valores a través de un valor de la aplicación en el [complemento de Maven de App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

O bien establezca las variables de entorno en la página **Configuración** > **Configuración de la aplicación** de Azure Portal.

A continuación, determine si el origen de datos debe estar disponible para una aplicación o para todas las aplicaciones que se ejecutan en el servlet de Tomcat.

#### <a name="application-level-data-sources"></a>Orígenes de datos de nivel de aplicación

1. Cree un archivo *context.xml* en el directorio *META-INF /* del proyecto. Si el directorio *META-INF/* no existe, créelo.

2. En *, context.xml*, agregue un elemento `Context` para vincular el origen de datos a una dirección JNDI. Reemplace el marcador de posición `driverClassName` por el nombre de clase de su controlador de la tabla anterior.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Actualice el archivo *web.xml* de la aplicación para que use el origen de datos de su aplicación.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Recursos de nivel de servidor compartidos

Para agregar un origen de datos compartido de nivel de servidor, será necesario editar el archivo server.xml de Tomcat. En primer lugar, cargue un [script de inicio](app-service-linux-faq.md#built-in-images) y establezca la ruta de acceso al script en **Configuración** > **Comando de inicio**. Puede cargar el script de inicio mediante [FTP](../deploy-ftp.md).

El script de inicio realizará una [transformación XSL](https://www.w3schools.com/xml/xsl_intro.asp) al archivo server.xml y generará el archivo XML resultante en `/usr/local/tomcat/conf/server.xml`. El script de inicio debe instalar libxslt a través de APK. El archivo XSL y el script de inicio se pueden cargar a través de FTP. A continuación se muestra un script de inicio de ejemplo.

```sh
# Install libxslt. Also copy the transform file to /home/tomcat/conf/
apk add --update libxslt

# Usage: xsltproc --output output.xml style.xsl input.xml
xsltproc --output /usr/local/tomcat/conf/server.xml /home/tomcat/conf/transform.xsl /home/tomcat/conf/server.xml
```

A continuación se puede ver un archivo XSL de ejemplo. Este archivo agrega un nuevo nodo de conector al archivo server.xml de Tomcat.

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="xml" indent="yes"/>

  <xsl:template match="@* | node()" name="Copy">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()"/>
    </xsl:copy>
  </xsl:template>

  <xsl:template match="@* | node()" mode="insertConnector">
    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template match="comment()[not(../Connector[@scheme = 'https']) and
                                 contains(., '&lt;Connector') and
                                 (contains(., 'scheme=&quot;https&quot;') or
                                  contains(., &quot;scheme='https'&quot;))]">
    <xsl:value-of select="." disable-output-escaping="yes" />
  </xsl:template>

  <xsl:template match="Service[not(Connector[@scheme = 'https'] or
                                   comment()[contains(., '&lt;Connector') and
                                             (contains(., 'scheme=&quot;https&quot;') or
                                              contains(., &quot;scheme='https'&quot;))]
                                  )]
                      ">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()" mode="insertConnector" />
    </xsl:copy>
  </xsl:template>

  <!-- Add the new connector after the last existing Connnector if there is one -->
  <xsl:template match="Connector[last()]" mode="insertConnector">
    <xsl:call-template name="Copy" />

    <xsl:call-template name="AddConnector" />
  </xsl:template>

  <!-- ... or before the first Engine if there is no existing Connector -->
  <xsl:template match="Engine[1][not(preceding-sibling::Connector)]"
                mode="insertConnector">
    <xsl:call-template name="AddConnector" />

    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template name="AddConnector">
    <!-- Add new line -->
    <xsl:text>&#xa;</xsl:text>
    <!-- This is the new connector -->
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" 
               maxThreads="150" scheme="https" secure="true" 
               keystroreFile="${{user.home}}/.keystore" keystorePass="changeit"
               clientAuth="false" sslProtocol="TLS" />
  </xsl:template>
  
</xsl:stylesheet>
```

#### <a name="finalize-configuration"></a>Finalización de la configuración

Por último, coloque los archivos JAR del controlador en la classpath de Tomcat y reinicie App Service.

1. Asegúrese de que los archivos del controlador JDBC estén disponibles para el cargador de clases de Tomcat. Para ello, colóquelos en el directorio */home/tomcat/lib*. (cree el directorio si no existe). Para cargar estos archivos en su instancia de App Service, realice los pasos siguientes:

    1. En el [Cloud Shell](https://shell.azure.com), instale la extensión webapp:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Ejecute el siguiente comando de la CLI para crear un túnel SSH desde el sistema local a App Service:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. Establezca la conexión al puerto de tunelización local con el cliente SFTP y cargue los archivos en la carpeta */home/tomcat/lib*.

    Como alternativa, puede usar un cliente FTP para cargar el controlador JDBC. Siga estas [instrucciones para obtener las credenciales de FTP](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

2. Si ha creado un origen de datos de nivel de servidor, reinicie la aplicación App Service de Linux. Tomcat restablecerá `CATALINA_BASE` a `/home/tomcat` y usará la configuración actualizadas.

### <a name="spring-boot"></a>Spring Boot

Para conectarse a orígenes de datos en aplicaciones de Spring Boot, se recomienda crear cadenas de conexión e insertarlas en su archivo *application.properties*.

1. En la sección "Configuración" de la página de App Service, establezca un nombre para la cadena, pegue la cadena de conexión de JDBC en el campo de valor y establezca el tipo en "Custom" (Personalizado). Opcionalmente, puede establecer esta cadena de conexión como configuración de ranura.

    Nuestra aplicación puede acceder a cadena de conexión como una variable de entorno denominada `CUSTOMCONNSTR_<your-string-name>`. Por ejemplo, la cadena de conexión que hemos creado anteriormente se denominará `CUSTOMCONNSTR_exampledb`.

2. En su archivo *application.properties*, haga referencia a esta cadena de conexión con el nombre de variable de entorno. En nuestro ejemplo, se usaría lo siguiente.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Consulte la [documentación de Spring Boot relativa al acceso a datos](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) y [a configuraciones externalizadas ](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) para más información acerca de este tema.

## <a name="configure-java-ee-wildfly"></a>Configuración de Java EE (WildFly)

> [!NOTE]
> Java Enterprise Edition en App Service Linux está actualmente en versión preliminar. Esta pila **no** se recomienda para l trabajo de producción.

Azure App Service en Linux permite a los desarrolladores de Java compilar, implementar y escalar sus aplicaciones Java Enterprise (Java EE) en un servicio basado en Linux totalmente administrado.  El entorno de tiempo de ejecución subyacente de Java Enterprise es el servidor de aplicaciones [WildFly](https://wildfly.org/) de código abierto.

Esta sección contiene las siguientes subsecciones:

- [Escalado con App Service](#scale-with-app-service)
- [Personalización de la configuración del servidor de aplicaciones](#customize-application-server-configuration)
- [Instalación de módulos y dependencias](#install-modules-and-dependencies)
- [Configuración de orígenes de datos](#configure-data-sources)
- [Habilitación de proveedores de mensajería](#enable-messaging-providers)

### <a name="scale-with-app-service"></a>Escalado con App Service

El servidor de aplicaciones WildFly, que se ejecuta en App Service en Linux, se ejecuta en modo independiente, no en una configuración de dominio. Al escalar horizontalmente el plan de App Service, cada instancia de WildFly se configura como un servidor independiente.

Escale la aplicación vertical u horizontalmente con [reglas de escalado](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) y [aumentando el número de instancias](../manage-scale-up.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="customize-application-server-configuration"></a>Personalización de la configuración del servidor de aplicaciones

Las instancias de Web App no tienen estado, por lo que todas las instancias nueva que se inicien deben configurarse en el inicio para que admitan la configuración de Wildfly que necesite la aplicación.
Puede escribir un script de Bash de inicio para llamar a la CLI de WildFly para:

- Configurar orígenes de datos
- Configurar proveedores de mensajería
- Agregar otros módulos y dependencias a la configuración del servidor de WildFly.

El script se ejecuta cuando WildFly está en funcionamiento, pero antes de que se inicie la aplicación. El script debe usar la [CLI de JBOSS](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) llamada desde */opt/jboss/wildfly/bin/jboss-cli.sh* para configurar el servidor de aplicaciones con cualquier configuración o cambios necesarios una vez que se inicie el servidor.

No use el modo interactivo de la CLI para configurar WildFly. En su lugar, puede proporcionar un script de comandos a la CLI de JBoss mediante el comando `--file`, por ejemplo:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Usar FTP para cargar el script de inicio en una ubicación de la instancia de App Service en el directorio */home* directorio, como */home/site/deployments/tools*. Para más información, consulte [Implementación de la aplicación en Azure App Service mediante FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

En el campo **Script de inicio** de Azure Portal seleccione la ubicación del script de shell de inicio, por ejemplo */home/site/deployments/tools/your-startup-script.sh*.

Especifique la [configuración de la aplicación](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) para pasar variables de entorno para su uso en el script. La configuración de la aplicación mantiene las cadenas de conexión y otros secretos necesarios para configurar la aplicación fuera del control de versiones.

### <a name="install-modules-and-dependencies"></a>Instalación de módulos y dependencias

Para instalar los módulos y sus dependencias en el classpath WildFly a través de la CLI de JBoss, deberá crear los siguientes archivos en su propio directorio. Algunos módulos y dependencias pueden necesitar una configuración adicional, como la nomenclatura de JNDI u otra configuración específica de la API, por lo que esta lista es un conjunto mínimo de lo que tendrá que configurar de una dependencia en la mayoría de los casos.

- Un [descriptor del módulo XML](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Este archivo XML define el nombre, los atributos y las dependencias del módulo. En este [archivo module.xml de ejemplo](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) se definen un módulo de Postgres, su dependencia JDBC del archivo JAR y otras dependencias del módulo necesarias.
- Las dependencias del archivo JAR necesarias para el módulo.
- Un script con los comandos de la CLI de JBoss para configurar el módulo nuevo. Este archivo contendrá los comandos que ejecutará la CLI de JBoss para configurar el servidor para usar la dependencia. Para obtener documentación sobre los comandos para agregar módulos, orígenes de datos y proveedores de mensajería, consulte [este documento](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- Un script de inicio de Bash para llamar a la CLI de JBoss y ejecutar el script del paso anterior. Este archivo se ejecutará cuando se reinicie la instancia de App Service o cuando se aprovisionen nuevas instancias durante un escalado horizontal. Este script de inicio es donde puede realizar otras configuraciones para la aplicación, ya que los comandos de JBoss se pasan a la CLI de JBoss. Como mínimo, este archivo puede ser un único comando para pasar el script de comandos de la CLI de JBoss a la CLI de JBoss:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Una vez que tenga los archivos y el contenido del módulo, siga estos pasos para agregar el módulo al servidor de aplicaciones de WildFly.

1. Usar FTP para cargar los archivos en una ubicación de la instancia de App Service en el directorio */home* directorio, como */home/site/deployments/tools*. Para más información, consulte [Implementación de la aplicación en Azure App Service mediante FTP/S](../deploy-ftp.md).
2. En la página **Configuración** > **Configuración general** de Azure Portal, en el campo **Script de inicio**, seleccione la ubicación del script de shell de inicio, por ejemplo */home/site/deployments/tools/startup.sh*.
3. Para reiniciar la instancia de App Service, presione el botón **Reiniciar** en la sección **Información general** del portal o mediante la CLI de Azure.

### <a name="configure-data-sources"></a>Configuración de orígenes de datos

Para configurar WildFly/JBoss para que accedan a un origen de datos, use el proceso general que se ha descrito en la sección "Instalación de módulos y dependencias". En la siguiente sección se proporcionan detalles específicos acerca de este proceso para orígenes de datos de SQL Server, MySQL y PostgreSQL.

En esta sección se da por supuesto que ya tiene una aplicación, una instancia de App Service y una instancia del servicio Azure Database. Las instrucciones siguientes hacen referencia a su nombre de App Service, su grupo de recursos y la información de la conexión de base de datos. Esta información se puede encontrar en Azure Portal.

Si prefiere realizar todo el proceso, desde el principio, mediante una aplicación de ejemplo, consulte [Tutorial: Compilación de una aplicación web Java EE y Postgres en Azure](tutorial-java-enterprise-postgresql-app.md).

Los pasos siguientes explican los requisitos para conectarse a una instancia de App Service existente y a la base de datos.

1. Descargue el controlador JDBC para [PostgreSQL](https://jdbc.postgresql.org/download.html), [MySQL](https://dev.mysql.com/downloads/connector/j/) o [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server). Desempaquete el archivo descargado para obtener el archivo. jar del controlador.

2. Cree un archivo con un nombre como *module.xml* y agregue el marcado siguiente. Reemplace el marcador de posición `<module name>` (incluidos los corchetes angulares) por `org.postgres` para PostgreSQL, `com.mysql` para MySQL o `com.microsoft` para SQL Server. Reemplace `<JDBC .jar file path>` por el nombre del archivo .jar del paso anterior, incluida la ruta de acceso completa a la ubicación en que colocará el archivo en la instancia de App Service. Puede ser cualquier ubicación dentro del directorio */home*.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

3. Cree un archivo con un nombre como *datasource commands.cli* y agregue el código siguiente. Reemplace `<JDBC .jar file path>` por el valor que usó en el paso anterior. Reemplace `<module file path>` por el nombre de archivo y ruta de acceso de App Service del paso anterior, por ejemplo */home/module.xml*.

    **PostgreSQL**

    ```console
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker

    reload --use-current-server-config=true
    ```

    **MySQL**

    ```console
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)

    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter

    reload --use-current-server-config=true
    ```

    **SQL Server**

    ```console
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)

    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter

    reload --use-current-server-config=true
    ```

    Este archivo lo ejecuta el script de inicio que se describe en el paso siguiente. Instala el controlador JDBC como un módulo WildFly, crea el origen de datos de WildFly correspondiente y vuelve a cargar el servidor para asegurarse de que los cambios surten efecto.

4. Cree un archivo con un nombre como *startup.sh* y agregue el código siguiente. Sustituya `<JBoss CLI script>` por el nombre del archivo que creó en el paso anterior. Asegúrese de que incluye la ruta de acceso completa a la ubicación en que colocará el archivo en su instancia de App Service, por ejemplo */home/datasource-commands.cli*.

    ```bash
    #!/usr/bin/env bash
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=<JBoss CLI script>
    ```

5. Use FTP para cargar el archivo .jar de JDBC, el archivo XML del módulo, el script de la CLI de JBoss y el script de inicio a su instancia de App Service. Coloque estos archivos en la ubicación que especificó en los pasos anteriores, como por ejemplo */home*. Para más información acerca de FTP, consulte [Implementación de la aplicación en Azure App Service mediante FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

6. Utilice la CLI de Azure para agregar una configuración a App Service que contenga la información de conexión de base de datos. Reemplace `<resource group>` y `<webapp name>` por los valores que usa App Service. Reemplace `<database server name>`, `<database name>`, `<admin name>` y `<admin password>` por la información de conexión de base de datos. La información de App Service y de la base de datos se puede obtener de Azure Portal.

    **PostgreSQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:postgresql://<database server name>:5432/<database name>?ssl=true \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **MySQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **SQL Server:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

    Los valores de DATABASE_CONNECTION_URL son diferentes para cada servidor de base de datos y también son diferentes de los valores de Azure Portal. Los formatos de dirección URL que se muestran aquí (y en los fragmentos de código anteriores) son necesarios para que los use WildFly:

    * **PostgreSQL:** `jdbc:postgresql://<database server name>:5432/<database name>?ssl=true`
    * **MySQL:** `jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT`
    * **SQL Server:** `jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;`

7. En Azure Portal, vaya a su App Service y busque la página **Configuración** > **Configuración general**. En el campo **Script de inicio**, seleccione el nombre y la ubicación del script de inicio, por ejemplo */home/startup.sh*.

La próxima vez que App Service se reinicie, ejecutará el script de inicio y dará los pasos de configuración necesarios. Para comprobar que esta configuración se realiza correctamente, puede acceder a App Service mediante SSH y, después, ejecutar el script de inicio usted mismo desde el símbolo del sistema de Bash. También puede examinar los registros de App Service. Para más información acerca de estas opciones, consulte [Registro y depuración de aplicaciones](#logging-and-debugging-apps).

A continuación, deberá actualizar la configuración de WildFly para la aplicación y volver a implementarla. Para ello, siga los pasos que se describen a continuación:

1. Abra el archivo *src/main/resources/META-INF/persistence.xml* de su aplicación y busque el elemento `<jta-data-source>`. Reemplace su contenido como se muestra aquí:

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

2. Vuelva a compilar e implementar la aplicación mediante el siguiente comando en el símbolo del sistema de Bash:

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

3. Para reiniciar la instancia de App Service, presione el botón **Reiniciar** en la sección **Información general** de Azure Portal o mediante la CLI de Azure.

La instancia de App Service ya está configurada para acceder a la base de datos.

Para más información acerca de cómo configurar la conectividad de una base de datos con WildFly, consulte [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource) o [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898).

### <a name="enable-messaging-providers"></a>Habilitación de proveedores de mensajería

Para habilitar Beans controlados por mensajes usando Service Bus como mecanismo de mensajería:

1. Use la [biblioteca de mensajería de Apache QPId JMS](https://qpid.apache.org/proton/index.html). Incluya esta dependencia en pom.xml (u otro archivo de compilación) de la aplicación.

2. Cree [Recursos de Service Bus](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Cree un espacio de nombres de Azure Service Bus, una cola dentro de ese espacio de nombres y una directiva de acceso compartido con funcionalidades de envío y recepción.

3. Pase la clave de la directiva de acceso compartido al código codificando la dirección URL de la clave principal de la directiva o [usando el SDK de Service Bus](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Siga los pasos descritos en la sección Módulos y dependencias con su descriptor XML de módulo, las dependencias de .jar, los comandos de la CLI de JBoss y el script de inicio para el proveedor de JMS. Además de los cuatro archivos, también deberá crear un archivo XML que defina el nombre JNDI de la cola de JMS y el tema. Consulte [este repositorio](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) para obtener los archivos de configuración de referencia.

## <a name="use-redis-as-a-session-cache-with-tomcat"></a>Uso de Redis como caché de sesión con Tomcat

Puede configurar Tomcat para que use un almacén de sesión externo como [Azure Redis Cache](/azure/azure-cache-for-redis/). Esto permite conservar el estado de sesión de usuario (como los datos del carro de la compra) cuando un usuario se transfiere a otra instancia de la aplicación, por ejemplo, cuando se produce un escalado automático, un reinicio o una conmutación por error.

Para usar Tomcat con Redis, debe configurar la aplicación para que use una implementación de [PersistentManager](http://tomcat.apache.org/tomcat-8.5-doc/config/manager.html). En los pasos siguientes se explica este proceso con [Pivotal Session Manager: redis-store](https://github.com/pivotalsoftware/session-managers/tree/master/redis-store) como ejemplo.

1. Abra un terminal de Bash y use `export <variable>=<value>` para establecer cada una de las siguientes variables de entorno.

    | Variable                 | Valor                                                                      |
    |--------------------------|----------------------------------------------------------------------------|
    | RESOURCEGROUP_NAME       | Nombre del grupo de recursos que contiene la instancia de App Service.       |
    | WEBAPP_NAME              | Nombre de la instancia de App Service.                                     |
    | WEBAPP_PLAN_NAME         | Nombre del plan de App Service.                                          |
    | REGIÓN                   | Nombre de la región donde se hospeda la aplicación.                           |
    | REDIS_CACHE_NAME         | Nombre de la instancia de Azure Redis Cache.                           |
    | REDIS_PORT               | Puerto SSL en el que escucha Redis Cache.                             |
    | REDIS_PASSWORD           | Clave de acceso principal de la instancia.                                  |
    | REDIS_SESSION_KEY_PREFIX | Valor que se especifica para identificar las claves de sesión que proceden de la aplicación. |

    Para encontrar la información de nombre, puerto y clave de acceso en Azure Portal, vea las secciones **Propiedades** o **Claves de acceso** de la instancia de servicio.

2. Cree o actualice el archivo *src/main/webapp/META-INF/context.xml* de la aplicación con el siguiente contenido:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Context path="">
        <!-- Specify Redis Store -->
        <Valve className="com.gopivotal.manager.SessionFlushValve" />
        <Manager className="org.apache.catalina.session.PersistentManager">
            <Store className="com.gopivotal.manager.redis.RedisStore"
                   connectionPoolSize="20"
                   host="${REDIS_CACHE_NAME}.redis.cache.windows.net"
                   port="${REDIS_PORT}"
                   password="${REDIS_PASSWORD}"
                   sessionKeyPrefix="${REDIS_SESSION_KEY_PREFIX}"
                   timeout="2000"
            />
        </Manager>
    </Context>
    ```

    Este archivo especifica y configura la implementación del administrador de sesión de la aplicación. Usa las variables de entorno establecidas en el paso anterior para mantener la información de la cuenta fuera de los archivos de código fuente.

3. Use FTP para cargar el archivo JAR del administrador de sesión en la instancia de App Service y colóquelo en el directorio */home/tomcat/lib*. Para más información, consulte [Implementación de la aplicación en Azure App Service mediante FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

4. Deshabilite la [cookie de afinidad de sesión](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) de la instancia de App Service. Puede hacerlo desde Azure Portal si va a la aplicación y establece **Configuración > Configuración general > Afinidad ARR** en **Desactivada**. También puede usar el siguiente comando:

    ```azurecli
    az webapp update -g <resource group> -n <webapp name> --client-affinity-enabled false
    ```

    De forma predeterminada, App Service usa las cookies de afinidad de sesión para asegurarse de que las solicitudes de los clientes con sesiones existentes se enrutan a la misma instancia de la aplicación. Este comportamiento predeterminado no requiere ninguna configuración, pero no puede conservar el estado de sesión de usuario cuando se reinicia la instancia de la aplicación o cuando el tráfico se vuelve a enrutar a otra instancia. Al [deshabilitar la configuración de afinidad de instancia de ARR existente](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) para desactivar el enrutamiento basado en cookies de sesión, permite que el almacén de sesiones configurado funcione sin interferencias.

5. Vaya a la sección **Propiedades** de la instancia de App Service y busque **Direcciones IP salientes adicionales**. Estas representan todas las posibles direcciones IP salientes de la aplicación. Cópielas para su uso en el paso siguiente.

6. Para cada dirección IP, cree una regla de firewall en la instancia de Azure Redis Cache. Puede hacerlo en Azure Portal en la sección **Firewall** de la instancia de Redis. Proporcione un nombre único para cada regla y establezca los valores **Dirección IP inicial** y **Dirección IP final** en la misma dirección IP.

7. Vaya a la sección **Configuración avanzada** de la instancia de Redis y establezca **Permitir el acceso solo mediante SSL** en **No**. Esto permite a la instancia de App Service comunicarse con Redis Cache a través de la infraestructura de Azure.

8. Actualice la configuración de `azure-webapp-maven-plugin` en el archivo *pom.xml* de la aplicación para hacer referencia a la información de la cuenta de Redis. Este archivo usa las variables de entorno establecidas anteriormente para mantener la información de la cuenta fuera de los archivos de código fuente.

    Si es necesario, cambie `1.7.0` a la versión actual del [Complemento Maven para Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

    ```xml
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.7.0</version>
        <configuration>

            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appServicePlanName>${WEBAPP_PLAN_NAME}-${REGION}</appServicePlanName>
            <appName>${WEBAPP_NAME}-${REGION}</appName>
            <region>${REGION}</region>
            <linuxRuntime>tomcat 9.0-jre8</linuxRuntime>

            <appSettings>
                <property>
                    <name>REDIS_CACHE_NAME</name>
                    <value>${REDIS_CACHE_NAME}</value>
                </property>
                <property>
                    <name>REDIS_PORT</name>
                    <value>${REDIS_PORT}</value>
                </property>
                <property>
                    <name>REDIS_PASSWORD</name>
                    <value>${REDIS_PASSWORD}</value>
                </property>
                <property>
                    <name>REDIS_SESSION_KEY_PREFIX</name>
                    <value>${REDIS_SESSION_KEY_PREFIX}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Xms2048m -Xmx2048m -DREDIS_CACHE_NAME=${REDIS_CACHE_NAME} -DREDIS_PORT=${REDIS_PORT} -DREDIS_PASSWORD=${REDIS_PASSWORD} IS_SESSION_KEY_PREFIX=${REDIS_SESSION_KEY_PREFIX}</value>
                </property>

            </appSettings>

        </configuration>
    </plugin>
    ```

9. Vuelva a compilar e implementar la aplicación.

    ```bash
    mvn package
    mvn azure-webapp:deploy
    ```

La aplicación ahora usa Redis Cache para la administración de sesiones.

Para obtener un ejemplo que puede usar para probar estas instrucciones, vea el repositorio [scaling-stateful-java-web-app-on-azure](https://github.com/Azure-Samples/scaling-stateful-java-web-app-on-azure) en GitHub.

## <a name="docker-containers"></a>Contenedores de Docker

Para utilizar el JDK de Zulu compatible con Azure en los contenedores, asegúrese de que extraer y usar las imágenes pregeneradas como se documenta en la [página de descarga de Azul Zulu Enterprise for Azure admitida](https://www.azul.com/downloads/azure-only/zulu/) o usar los ejemplos `Dockerfile` del [repositorio de GitHub de Java de Microsoft](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Instrucción de compatibilidad

### <a name="runtime-availability"></a>Disponibilidad de runtime

App Service para Linux es compatible con dos entornos de ejecución para el hospedaje administrado de aplicaciones web Java:

- El [contenedor de servlets Tomcat](https://tomcat.apache.org/) para ejecutar aplicaciones empaquetadas como archivos WAR (archivo web). Las versiones compatibles son 8.5 y 9.0.
- Java SE Runtime Environment para ejecutar aplicaciones como archivos JAR (archivo Java). Las versiones compatibles actuales son Java 8 y 11.

### <a name="jdk-versions-and-maintenance"></a>Mantenimiento y versiones de JDK

Las compilaciones del OpenJDK de Azul Zulu Enterprise son una distribución gratuita, multiplataforma y lista para producción del OpenJDK para Azure y Azure Stack respaldado por Microsoft y Azul Systems. Contienen todos los componentes para la creación y ejecución de aplicaciones de Java SE. El JDK se puede instalar desde [Instalación del JDK de Java](https://aka.ms/azure-jdks).

Los JDK compatibles se revisarán trimestralmente de manera automática en enero, abril, julio y octubre de cada año.

### <a name="security-updates"></a>Actualizaciones de seguridad

Las revisiones y correcciones de las vulnerabilidades de seguridad principales se publicarán en cuanto estén disponibles a través de Azul Systems. Una vulnerabilidad "principal" se define con una puntuación total de 9.0 o superior en el [CVSS (Common Vulnerability Scoring System), versión 2 del NIST](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Desuso y retirada

Si un entorno compatible de Java Runtime se va a retirar, los desarrolladores de Azure que lo usen recibirán un aviso de desuso al menos seis meses antes de su retirada.

## <a name="next-steps"></a>Pasos siguientes

Visite el centro de [Azure para desarrolladores de Java](/java/azure/) para encontrar guías de inicio rápido de Azure, tutoriales y documentación de referencia de Java.

Encontrará respuestas a las preguntas generales sobre el uso de App Service para Linux que no son específicas del desarrollo de Java en las [Preguntas más frecuentes sobre Azure App Service en Linux](app-service-linux-faq.md).
