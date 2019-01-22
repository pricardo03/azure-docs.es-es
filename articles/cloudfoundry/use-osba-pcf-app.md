---
title: Uso de Open Service Broker para bases de datos de Azure con una aplicación en Pivotal Cloud Foundry
author: zr-msft
manager: jeconnoc
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 01/11/2019
ms.topic: tutorial
description: Este artículo explica cómo configurar una aplicación de Pivotal Cloud Foundry para que use una instancia de Open Service Broker para bases de datos de Azure
keywords: Pivotal Cloud Foundry, Cloud Foundry, Open Service Broker, Open Service Broker para Azure
tags: Cloud-Foundry
ms.openlocfilehash: d553cd09ef42e47e3a10fb96039063b8aae665cb
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258883"
---
# <a name="tutorial-use-an-open-service-broker-for-azure-database-with-an-application-on-pivotal-cloud-foundry"></a>Tutorial: Uso de Open Service Broker para bases de datos de Azure con una aplicación en Pivotal Cloud Foundry

El uso de Open Service Broker para Azure con una instancia de Pivotal Cloud Foundry le permite aprovisionar servicios como, por ejemplo, bases de datos, directamente en Azure desde la CLI de Cloud Foundry y la instancia de Pivotal Cloud Foundry. También puede enlazar esos servicios a una aplicación que se ejecute en la instancia de Pivotal Cloud Foundry. Si enlaza una aplicación a un servicio de esta manera, no tiene que actualizar ningún código ni configuración en la aplicación. En este artículo se explica cómo usar Open Service Broker para una base de datos de Azure con una aplicación en Pivotal Cloud Foundry.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Preparar el espacio de aplicaciones en Pivotal Cloud Foundry.
> * Clonar el origen de una aplicación de ejemplo desde GitHub.
> * Preparar la aplicación para la implementación.
> * Implemente la aplicación.
> * Crear una base de datos mediante Open Service Broker para Azure.
> * Enlazar la base de datos a la aplicación.

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, debe:

* [Tener Pivotal Cloud Foundry instalado y configurado](create-cloud-foundry-on-azure.md)
* [Tener Open Service Broker para Azure instalado y configurado](https://network.pivotal.io/products/azure-open-service-broker-pcf) con su instancia de Cloud Foundry

Este es un ejemplo de la pantalla de administración de operaciones de Pivotal Cloud Foundry con el icono de Open Service Broker para Azure instalado y configurado:

![Pivotal Cloud Foundry con Open Service Broker para Azure instalado](media/pcf-ops-manager.png)

## <a name="prepare-your-application-space-in-pivotal-cloud-foundry"></a>Preparación del espacio de aplicaciones en Pivotal Cloud Foundry

Para implementar la aplicación en la instancia de Pivotal Cloud Foundry, debe iniciar sesión con la herramienta de línea de comandos `cf`. También debe seleccionar la organización y el espacio de destino que desee.

Para comprobar que ha iniciado sesión y mostrar el espacio que ha seleccionado como destino, use `cf target`. En el siguiente ejemplo, el usuario ya ha iniciado sesión como *administrador*, ha empleado la organización *myorg* y ha seleccionado como destino el espacio *dev*:

```cmd
$ cf target

api endpoint:   https://api.system.40.85.111.222.cf.pcfazure.com
api version:    2.120.0
user:           admin
org:            myorg
space:          dev
```

Para iniciar sesión, use `cf login`:

```cmd
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Para crear una nueva organización y otro espacio, use `cf create-org` y `cf create-space`:

```cmd 
cf create-org myorg
cf create-space dev -o myorg
```

Para seleccionar como destino un espacio, use `cf target`:

```cmd
cf target -o myorg -s dev
```

## <a name="get-application-code"></a>Obtención del código de la aplicación

Para que una aplicación use Open Service Broker para Azure, la aplicación debe utilizar un origen de datos como, por ejemplo, una base de datos. En este artículo, se usará la [aplicación de ejemplo Spring Music de Cloud Foundry](https://github.com/cloudfoundry-samples/spring-music) para mostrar una aplicación que usa un origen de datos.

Clone la aplicación desde GitHub y vaya a su directorio:

```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring music
```

> [!TIP]
> Para ver los orígenes de datos de esta aplicación, abra el archivo [src/main/resources/application.yml](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/application.yml).


## <a name="prepare-the-application-for-deployment"></a>Preparación de la aplicación para la implementación

Antes de implementar la aplicación en la instancia de Pivotal Cloud Foundry, deberá compilarla. Con fines ilustrativos, se va a habilitar algún registro de *DEBUG* que permitirá registrar la información de la conexión del origen de datos.

Para habilitar el registro de *DEBUG* para obtener los detalles de conexión de la base de datos, agregue la siguiente propiedad yaml al final de *application.yml*:

```yaml
---
logging:
  level:
    com.zaxxer.hikari: DEBUG
```

La aplicación de ejemplo usa Gradle para compilar la aplicación en un archivo jar ejecutable de Spring Boot. El archivo jar ejecutable se implementará en la instancia de Pivotal Cloud Foundry. Para compilar la aplicación:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```


## <a name="deploy-your-application"></a>Implementación de aplicación

Use el comando `cf push` para implementar la aplicación en la instancia de Pivotal Cloud Foundry:

```cmd
cf push

Pushing from manifest to org myorg / space dev as admin...
Using manifest file /path/to/spring-music/manifest.yml
Getting app info...
Creating app with these attributes...
+ name:       spring-music
...
Waiting for app to start...

name:              spring-music
requested state:   started
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
     state     since                  cpu    memory         disk           details
#0   running   2018-12-06T21:24:06Z   0.0%   313.3M of 1G   170.7M of 1G
```

Copie el valor de *routes* que aparece en la salida `cf push`. La ruta es la dirección URL que usará para acceder a la aplicación en ejecución. Por ejemplo: 

```cmd
...
routes:            spring-music-wacky-oribi.app.40.85.111.222.cf.pcfazure.com
...
```


Una vez implementada la aplicación, puede ver los registros de la aplicación para conocer la dirección URL de conexión que emplea la aplicación. El comando siguiente muestra los registros de la aplicación y utiliza `grep` para buscar la configuración de *jdbcUrl*.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
```

Observe que la aplicación usa *h2:mem:testdb*, que es la base de datos en memoria. Se configura automáticamente una aplicación de Spring para que use una base de datos en memoria si hay una dependencia de base de datos en memoria en classpath y la [configuración automática](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-auto-configuration.html) está habilitada. La aplicación de ejemplo tiene la [dependencia de base de datos en memoria h2 configurada](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L49) y la configuración automática habilitada en [src/main/java/org/cloudfoundry/samples/music/Application.java](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/java/org/cloudfoundry/samples/music/Application.java#L8).

Use ruta de la aplicación para ir allí mediante un explorador. La ruta, o dirección URL, aparece en la salida del comando `cf push`.

> [!TIP]
> También puede mostrar la dirección URL de la aplicación mediante la ejecución de `cf apps`.

Una vez que vaya a la aplicación mediante el explorador, interactúe con esta mediante la eliminación de algunos álbumes existentes y la creación de otros nuevos. La aplicación de ejemplo usa la base de datos en memoria para guardar los cambios. Puede observar también que la aplicación se ha rellenado con algunos [datos predeterminados](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json). 

![Aplicación Spring Music con datos predeterminados](media/music-app.png)

Como la aplicación usa una base de datos en memoria, los cambios no persistirán si la aplicación se reinicia o se vuelve a implementar. Para mostrar que los cambios no han persistido, después de efectuar algunos cambios, vuelva a realizar una copia intermedia de la aplicación mediante `cf restage`:

```cmd
cf restage spring-music
```

Una vez realizada esta copia intermedia, vaya a ella en un explorador mediante la misma dirección URL. Observe que los cambios que efectuó no están y que aparecen los datos predeterminados.

![Aplicación Spring Music con datos predeterminados](media/music-app.png)

## <a name="create-a-database"></a>Creación de una base de datos

Para crear una base de datos persistente en Azure mediante Open Service Broker, use el comando `cf create-service`. El siguiente comando aprovisiona una base de datos de PostgreSQL en Azure en el grupo de recursos *MyResourceGroup* de la región *eastus*. Hay más información disponible sobre *resourceGroup*, *location* y otros parámetros JSON específicos de Azure en la [documentación de referencia del módulo PostgreSQL](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md#provision):

```cmd
cf create-service azure-postgresql-9-6 general-purpose mypgsql -c '{"resourceGroup":"MyResourceGroup", "location":"eastus"}'
```

La base de datos se expone en la instancia de Pivotal Cloud Foundry como un servicio denominado *mypgsql*. Una vez que la base de datos ha completado el aprovisionamiento, lo cual debería tardar unos minutos, se puede enlazar con la aplicación. Para comprobar que la base de datos ha completado correctamente el aprovisionamiento, use el comando: `cf services`

```cmd
cf services

Getting services in org myorg / space dev as admin...

name      service                plan              bound apps     last operation
mypgsql   azure-postgresql-9-6   general-purpose                  create succeeded
```

El valor *última operación* del servicio será *create succeeded* (Creación correcta) si se ha completado el aprovisionamiento.

## <a name="bind-the-database-to-your-application"></a>Enlazar la base de datos a la aplicación

Use el comando `bind-service` para enlazar el servicio a la aplicación.

```cmd
cf bind-service spring-music mypgsql
```

Después de enlazar el servicio a la aplicación, deberá realizar una copia intermedia de la aplicación para que los cambios surtan efecto.

```cmd
cf restage spring-music
```

La aplicación está [configurada para usar Spring Cloud Connectors](https://github.com/cloudfoundry-samples/spring-music/blob/master/build.gradle#L45...L46), lo cual permite a la instancia de Pivotal Cloud Foundry usar la [reconfiguración automática](https://docs.cloudfoundry.org/buildpacks/java/configuring-service-connections/spring-service-bindings.html#auto) en el origen de datos de la aplicación. En este caso, la instancia de Pivotal Cloud Foundry reconfigurará automáticamente la aplicación para que esta use el servicio al que está enlazado para un origen de datos cuando se realice la copia intermedia de la aplicación.

Una vez que se haya realizado la copia intermedia de la aplicación, esta usará *mypgsql* para almacenar los datos en lugar de la base de datos en memoria.

Ahora, todos los cambios que realice persistirán a pesar de los reinicios y las nuevas implementaciones. También puede ver la dirección URL de conexión que está usando la aplicación si mira los registros de la aplicación de nuevo.

```cmd
cf logs spring-music --recent | grep jdbcUrl

2018-12-07T14:44:30.57-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:44:30.574 DEBUG 24 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
2018-12-07T14:48:58.10-0600 [APP/PROC/WEB/0] OUT 2018-12-07 20:48:58.107 DEBUG 16 --- [           main] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:postgresql://12345678-aaaa-bbbb-cccc-1234567890ab.postgres.database.azure.com:5432/123456789?user=aabbcc1122@12345678-aaaa-bbbb-cccc-1234567890ab&password=<masked>&&sslmode=require
```

Observe que hay dos entradas:

* El valor original de *h2:mem:testdb*.
* El valor actualizado de la base de datos de PostgreSQL cuando se realizó la copia intermedia de la aplicación.

Para comprobar que los datos persisten en la base de datos de PostgreSQL, vaya a la aplicación en el explorador, efectúe algunos cambios y, después, realice una copia intermedia de la aplicación:

```cmd
cf restage spring-music
```

Una vez realizada esta copia intermedia, vaya a ella en un explorador mediante la misma dirección URL. Observe que los cambios que ha realizado siguen estando allí.

## <a name="cleanup"></a>Limpieza

Si desea desconectar la aplicación de la base de datos, puede desenlazarla mediante el comando `cf unbind-service`.

```cmd
cf unbind-service spring-music mypgsql
```

Al igual que en el caso del enlace de la aplicación a un servicio, debe realizar una copia intermedia de la aplicación para que estos cambios surtan efecto. Esta acción dejará a *mypgsql* y a la aplicación intactos, pero la aplicación empezará a usar la base de datos en memoria en lugar de *mypgsql*.

Para eliminar la base de datos, puede usar el comando `cf delete-service`. *Esta acción no se puede deshacer, así que asegúrese de que desea eliminar la base de datos antes de continuar.*

```cmd
cf delete-service mypgsql
```

Para eliminar la aplicación de la instancia de Pivotal Cloud Foundry:
 
```cmd
cf delete spring-music
```

## <a name="next-steps"></a>Pasos siguientes

Este tutorial describió la implementación de una aplicación en Pivotal Cloud Foundry, así como la creación de una base de datos mediante Open Service Broker para Azure. También explicó el enlace de la base de datos a la aplicación en la instancia de Pivotal Cloud Foundry. Para más información sobre la implementación de aplicaciones en Cloud Foundry en Azure, consulte:

* [Cloud Foundry en Azure](../virtual-machines/linux/cloudfoundry-get-started.md)
* [Implementación de la primera aplicación en Cloud Foundry en Microsoft Azure](../virtual-machines/linux/cloudfoundry-deploy-your-first-app.md)