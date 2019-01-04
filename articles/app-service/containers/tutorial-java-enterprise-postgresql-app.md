---
title: 'Compilación de la aplicación web de Java Enterprise en Linux: Azure App Service | Microsoft Docs'
description: Empiece a trabajar con una aplicación de Java Enterprise en Wildfly en Azure App Service en Linux.
author: JasonFreeberg
manager: routlaw
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 11/13/2018
ms.author: jafreebe
ms.custom: seodec18
ms.openlocfilehash: 3a668783e8257ef9074d12b30ff0afc3a40325f4
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539731"
---
# <a name="tutorial-build-a-java-ee-and-postgres-web-app-in-azure"></a>Tutorial: Compilación de una aplicación web Java EE y Postgres en Azure

Este tutorial le mostrará cómo crear una aplicación web de Java Enterprise Edition (EE) en Azure App Service y cómo conectarla a una base de datos de Postgres. Cuando haya terminado, tendrá una aplicación [WildFly](https://www.wildfly.org/about/) que almacena datos en [Azure Database for Postgres](https://azure.microsoft.com/services/postgresql/) y que se ejecuta en Azure [App Service para Linux](app-service-linux-intro.md).

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Implementar una aplicación de Java EE en Azure con Maven
> * Crear una base de datos de Postgres en Azure
> * Configurar el servidor de WildFly para que use Postgres
> * Actualización de la aplicación y nueva implementación
> * Ejecutar pruebas unitarias en WildFly

## <a name="prerequisites"></a>Requisitos previos

1. [Descarga e instalación de Git](https://git-scm.com/)
2. [Descargue e instale Maven 3](https://maven.apache.org/install.html)
3. [Descargue e instale la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="clone-and-edit-the-sample-app"></a>Clonación y edición de la aplicación de ejemplo

En este paso, va a clonar la aplicación de ejemplo y a configurar el modelo de objetos de proyecto (POM o pom.xml) de Maven para la implementación.

### <a name="clone-the-sample"></a>Clonación del ejemplo

En la ventana de terminal, vaya a un directorio de trabajo y clone [el repositorio de ejemplo](https://github.com/Azure-Samples/wildfly-petstore-quickstart).

```bash
git clone https://github.com/Azure-Samples/wildfly-petstore-quickstart.git
```

### <a name="update-the-maven-pom"></a>Actualización del archivo POM de Maven

Actualice el archivo POM de Maven con el grupo de recursos y el nombre deseado de la instancia de App Service. Estos valores se insertarán en el complemento de Azure, que aparece más abajo en el archivo _pom.xml_. No es necesario crear el plan o la instancia de App Service de antemano. El complemento de Maven creará el grupo de recursos y la instancia de App Service si aún no existen.

Puede desplazarse hacia abajo a la sección `<plugins>` de _pom.xml_ para inspeccionar el complemento de Azure. La sección de la configuración de `<plugin>` en el archivo _pom.xml_ debe azure-webapp-maven-plugin debe incluir la siguiente configuración:

```xml
      <!--*************************************************-->
      <!-- Deploy to WildFly in App Service Linux           -->
      <!--*************************************************-->
 
      <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.5.0</version>
        <configuration>
 
          <!-- Web App information -->
          <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
          <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
          <appName>${WEBAPP_NAME}</appName>
          <region>${REGION}</region>
 
          <!-- Java Runtime Stack for Web App on Linux-->
          <linuxRuntime>wildfly 14-jre8</linuxRuntime>
 
        </configuration>
      </plugin>
```

Reemplace los marcadores de posición por los nombres de recurso que desee:
```xml
<azure.plugin.appname>YOUR_APP_NAME</azure.plugin.appname>
<azure.plugin.resourcegroup>YOUR_RESOURCE_GROUP</azure.plugin.resourcegroup>
```


## <a name="build-and-deploy-the-application"></a>Compilar e implementar la aplicación

Ahora usaremos Maven para compilar la aplicación e implementarla en App Service.

### <a name="build-the-war-file"></a>Compilación del archivo .war

El archivo POM de este proyecto está configurado para empaquetar la aplicación en un archivo web (WAR). Compile la aplicación con Maven:

```bash
mvn clean install -DskipTests
```

Los casos de prueba en esta aplicación están diseñados para ejecutarse cuando la aplicación se implementa en WildFly. Vamos a omitir las pruebas para compilar de forma local y las ejecutará una vez que la aplicación se implemente en App Service.

### <a name="deploy-to-app-service"></a>Implementación en App Service

Ahora que el archivo WAR está listo, podemos usar el complemento de Azure para realizar la implementación en App Service:

```bash
mvn azure-webapp:deploy
```

Cuando finalice la implementación, continúe con el paso siguiente.

### <a name="create-a-record"></a>Creación de un registro

Abra un explorador y vaya a `https://<your_app_name>.azurewebsites.net/`. Enhorabuena, ha implementado una aplicación de Java EE en Azure App Service.

En este momento, la aplicación usa una base de datos H2 en memoria. Haga clic en "Admin." en la barra de navegación y cree una nueva categoría. El registro de la base de datos en memoria se perderá si reinicia la instancia de App Service. En los pasos siguientes corregirá esto mediante el aprovisionamiento de una base de datos de Postgres en Azure y la configuración de WildFly para que la use.

![Ubicación del botón Admin.](media/tutorial-java-enterprise-postgresql-app/admin_button.JPG)

## <a name="provision-a-postgres-database"></a>Aprovisionamiento de una base de datos de Postgres

Para aprovisionar un servidor de bases de datos de Postgres, abra un terminal y ejecute el comando siguiente con los valores deseados para el nombre del servidor, nombre de usuario, contraseña y ubicación. Use el mismo grupo de recursos en el que se encuentra la instancia de App Service. Tome nota de la contraseña para su uso posterior.

```bash
az postgres server create -n <desired-name> -g <same-resource-group> --sku-name GP_Gen4_2 -u <desired-username> -p <desired-password> -l <location>
```

Vaya al portal y busque la base de datos de Postgres. Cuando la hoja esté activa, copie los valores de "Nombre del servidor" y "Nombre de inicio de sesión del administrador del servidor" ya que los necesitará más adelante.

### <a name="allow-access-to-azure-services"></a>Permitir el acceso a servicios de Azure

En el panel **Seguridad de la conexión** de la hoja de Azure Database, cambie el botón "Permitir el acceso a servicios de Azure" a la posición **ON**.

![Permitir acceso a los servicios de Azure](media/tutorial-java-enterprise-postgresql-app/postgress_button.JPG)

## <a name="update-your-java-app-for-postgres"></a>Actualización de la aplicación Java para Postgres

Ahora vamos a realizar algunos cambios a la aplicación Java para que pueda usar la base de datos de Postgres.

### <a name="add-postgres-credentials-to-the-pom"></a>Agregar credenciales de Postgres al archivo POM

En _pom.xml_ reemplace los valores de los marcadores de posición por su nombre de servidor de Postgres, el nombre de inicio de sesión de administrador y la contraseña. Estos valores se insertarán como variables de entorno en la instancia de App Service al volver a implementar la aplicación.

```xml
<azure.plugin.postgres-server-name>SERVER_NAME</azure.plugin.postgres-server-name>
<azure.plugin.postgres-username>USERNAME@FIRST_PART_OF_SERVER_NAME</azure.plugin.postgres-username>
<azure.plugin.postgres-password>PASSWORD</azure.plugin.postgres-password>
```

### <a name="update-the-java-transaction-api"></a>Actualización de Java Transaction API

A continuación, se debe editar la configuración de Java Transaction API (JTA) para que nuestra aplicación Java se comunique con Postgres en lugar de con la base de datos H2 en memoria que usamos anteriormente. Abra un editor para _src/main/resources/META-INF/persistence.xml_. Reemplace el valor de `<jta-data-source>` con `java:jboss/datasources/postgresDS`. La configuración del archivo XML de JTA debería ser esta:

```xml
...
<jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
...
```

## <a name="configure-the-wildfly-application-server"></a>Configuración del servidor de aplicaciones WildFly

Antes de implementar la aplicación con la configuración actualizada, se debe actualizar el servidor de aplicaciones WildFly con el módulo de Postgres y sus dependencias. Para configurar el servidor, se necesitan los cuatro archivos del directorio `wildfly_config/`:

- **postgresql-42.2.5.jar**: Este archivo JAR es el controlador JDBC para Postgres. Para más información, consulte el [sitio web oficial](https://jdbc.postgresql.org/index.html).
- **postgres-module.xml**: Este archivo XML declara un nombre para el módulo de Postgres (org.postgres). También especifica los recursos y las dependencias necesarios para el módulo que se va a usar.
- **jboss_cli_commands.cl**: Este archivo contiene los comandos de configuración que ejecutará la CLI de JBoss. Los comandos agregan el módulo de Postgres al servidor de aplicaciones WildFly, proporcionan las credenciales, declaran un nombre JNDI, establecen el umbral de tiempo de espera, etc. Si no está familiarizado con la CLI de JBoss, consulte la [documentación oficial](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- **startup_script.sh**: Por último, este script de shell se ejecutará cada vez que se inicie la instancia de App Service. El script realiza solo una función: la canalización de los comandos de `jboss_cli_commands.cli` a la CLI de JBoss.

Se recomienda encarecidamente leer el contenido de estos archivos, especialmente el de _jboss_cli_commands.cli_.

### <a name="ftp-the-configuration-files"></a>Envío por FTP de los archivos de configuración

Es necesario enviar por FTP el contenido de `wildfly_config/` a la instancia de App Service. Para obtener las credenciales de FTP, haga clic en el botón **Obtener perfil de publicación** en la hoja de App Service en Azure Portal. El nombre de usuario y la contraseña del FTP estará en el documento XML descargado. Para más información sobre el perfil de publicación, consulte [este documento](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials).

Mediante la herramienta de FTP que prefiera, transfiera los cuatro archivos de `wildfly_config/` a `/home/site/deployments/tools/`. (Tenga en cuenta que no debe transferir el directorio, solo los propios archivos).


### <a name="finalize-app-service"></a>Finalización de App Service

Vaya al panel "Configuración de la aplicación" en la hoja de App Service. En "Tiempo de ejecución", establezca el campo "Archivo de inicio" en `/home/site/deployments/tools/startup_script.sh`. Esto garantiza que el script de shell se ejecutará después de crear la instancia de App Service, pero antes de que el servidor WildFly se inicie.

Finalmente, reinicie App Service. El botón está en el panel "Información general".

## <a name="redeploy-the-application"></a>Volver a implementar la aplicación

En una ventana del terminal, recompile y vuelva a implementar la aplicación.

```bash
mvn clean install -DskipTests azure-webapp:deploy
```

Felicidades. La aplicación ya usa una base de datos de Postgres y todos los registros que se crean se almacenan en Postgres, en lugar de en la base de datos H3 en memoria que se empleaba anteriormente. Para confirmarlo, puede realizar un registro y reiniciar App Service. El registro seguirá estando allí cuando reinicie la aplicación.

## <a name="clean-up"></a>Limpieza

Si no necesita estos recursos para otro tutorial (consulte Pasos siguientes), puede ejecutar el siguiente comando para eliminarlos:

```bash
az group delete --name <your_resource_group> 
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una aplicación de Java EE implementada en App Service, consulte la [guía para desarrolladores de Java Enterprise](https://aka.ms/wildfly-quickstart) para más información sobre cómo configurar servicios, solucionar problemas y escalar la aplicación.