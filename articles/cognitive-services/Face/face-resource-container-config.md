---
title: Configuración de contenedores
titlesuffix: Face - Cognitive Services - Azure
description: Opciones de configuración de los contenedores.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: bfda7a82aeff97f560377864769a4c5dd6c03ff3
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634935"
---
# <a name="configure-containers"></a>Configuración de contenedores

El contenedor de Face usa un marco de configuración común, por lo que puede configurar y administrar fácilmente la configuración de almacenamiento, registro, telemetría y seguridad de los contenedores.

## <a name="configuration-settings"></a>Valores de configuración

Las opciones de configuración del contenedor de Face son jerárquicas y todos los contenedores usan una jerarquía compartida, que se basa en la siguiente estructura de nivel superior:

* [ApiKey](#apikey-configuration-setting)
* [Application Insights](#applicationinsights-configuration-settings)
* [Autenticación](#authentication-configuration-settings)
* [Facturación](#billing-configuration-setting)
* [CloudAI](#cloudai-configuration-settings)
* [Eula](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [Registro](#logging-configuration-settings)
* [Mounts](#mounts-configuration-settings)

Puede usar [variables de entorno](#configuration-settings-as-environment-variables) o [argumentos de la línea de comandos](#configuration-settings-as-command-line-arguments) para especificar las opciones de configuración al crear una instancia de un contenedor de Face.

Los valores de variable de entorno invalidan los valores de argumento de la línea de comandos, que a su vez invalidan los valores predeterminados para la imagen de contenedor. En otras palabras, si especifica valores diferentes en una variable de entorno y un argumento de la línea de comandos para la misma opción de configuración, tales como `Logging:Disk:LogLevel`, y, a continuación, crea una instancia de un contenedor, el contenedor del que se creó la instancia usa el valor de la variable de entorno.

### <a name="configuration-settings-as-environment-variables"></a>Opciones de configuración como variables de entorno

Puede usar la [sintaxis de variables de entorno de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#configuration-by-environment) para especificar las opciones de configuración.

El contenedor lee las variables de entorno del usuario cuando se crea una instancia del contenedor. Si existe una variable de entorno, su valor invalida el valor predeterminado para la opción de configuración especificada. La ventaja de usar variables de entorno es que se pueden establecer varias opciones de configuración antes de crear instancias de contenedores y varios contenedores pueden usar automáticamente el mismo conjunto de opciones de configuración.

Por ejemplo, los siguientes comandos usan una variable de entorno para configurar el nivel de registro de consola en [LogLevel.Information](https://msdn.microsoft.com) y, a continuación, se crea una instancia de un contenedor de la imagen de contenedor de Face. El valor de la variable de entorno invalida la opción de configuración predeterminada.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Opciones de configuración como argumentos de la línea de comandos

Puede usar la [sintaxis de argumento de la línea de comandos de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) para especificar las opciones de configuración.

Puede especificar las opciones de configuración en el parámetro `ARGS` opcional del comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) que se usa para crear una instancia de un contenedor a partir de una imagen de contenedor descargada. La ventaja de utilizar argumentos de la línea de comandos es que cada contenedor puede usar un conjunto de opciones de configuración diferente y personalizado.

Por ejemplo, los siguientes comandos crean una instancia de un contendedor a partir de la imagen de contenedor de Face y se configura el nivel de registro de consola en LogLevel.Information, lo que invalida la opción de configuración predeterminada.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>Opción de configuración ApiKey

La opción de configuración `ApiKey` especifica la clave de configuración del recurso de Face en Azure que se usa para realizar un seguimiento de la información de facturación del contenedor. Debe especificar un valor para esta opción de configuración que debe ser una clave de configuración válida para el recurso de Face especificado para la opción de configuración [`Billing`](#billing-configuration-setting).

> [!IMPORTANT]
> Las opciones de configuración [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) y [`Eula`](#eula-configuration-setting) se usan en conjunto, y debe proporcionar los valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](face-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Opción de configuración ApplicationInsights

Los valores de configuración de la sección `ApplicationInsights` le permiten agregar la compatibilidad de telemetría con [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor. Azure Application Insights proporciona una supervisión detallada del contenedor hasta el nivel de código. Puede supervisar fácilmente la disponibilidad, el rendimiento y el uso del contenedor. También puede identificar y diagnosticar errores en el contenedor rápidamente sin tener que esperar a que un usuario informe de ellos.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `ApplicationInsights`.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `InstrumentationKey` | string | Clave de instrumentación de la instancia de Application Insights para la que se envían los datos de telemetría del contenedor. Para más información, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Opción de configuración Authentication

Las opciones de configuración `Authentication` proporcionan opciones de seguridad de Azure para el contenedor. Aunque las opciones de configuración de esta sección están disponibles, el contenedor de Face no utiliza esta sección.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Storage` | string | Clave de instrumentación de la instancia de Application Insights para la que se envían los datos de telemetría del contenedor. Para más información, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="billing-configuration-setting"></a>Opción de configuración Billing

La opción de configuración `Billing` especifica el URI del punto de conexión del recurso de Face en Azure que se usa para realizar un seguimiento de la información de facturación del contenedor. Debe especificar un valor para esta opción de configuración y el valor debe ser un URI de punto de conexión válido para un recurso de Face en Azure.

> [!IMPORTANT]
> Las opciones de configuración [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) y [`Eula`](#eula-configuration-setting) se usan en conjunto, y debe proporcionar los valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](face-how-to-install-containers.md#billing).

## <a name="cloudai-configuration-settings"></a>Opciones de configuración CloudAI

Las opciones de configuración de la sección `CloudAI` proporcionan opciones específicas de contenedor únicas para el contenedor. Se admiten las opciones de configuración y objetos siguientes para el contenedor de Face en la sección `CloudAI`.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Storage` | Objeto | Escenario de almacenamiento utilizado por el contenedor de Face. Para obtener más información sobre los escenarios de almacenamiento y la configuración asociada para el objeto `Storage`, consulte [Configuración del escenario de almacenamiento](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>Configuración del escenario de almacenamiento

El contenedor de Face almacena datos de blob, de caché, de metadatos y de la cola, en función de lo que se va a almacenar. Por ejemplo, los índices de aprendizaje y los resultados de un grupo grande de personas se almacenan como datos de blob. El contenedor de Face proporciona dos escenarios de almacenamiento diferentes al interactuar con estos tipos de datos y almacenarlos:

* Memoria  
  Los cuatro tipos de datos se almacenan en la memoria. No se distribuyen ni son persistentes. Si el contenedor de Face se detiene o elimina, se destruyen todos los datos en el almacenamiento de dicho contenedor.  
  Se trata del escenario de almacenamiento predeterminado para el contenedor de Face.
* Azure  
  El contenedor de Face utiliza Azure Storage y Azure Cosmos DB para distribuir estos cuatro tipos de datos en el almacenamiento persistente. Los datos de blob y de cola se controlan mediante Azure Storage. Los datos de la memoria caché y los metadatos se controlan mediante Azure Cosmos DB, con MongoDB API. Si el contenedor de Face se detiene o elimina, todos los datos en el almacenamiento de dicho contenedor permanecen almacenados en Azure Storage y Azure Cosmos DB.  
  Los recursos utilizados por el escenario de Azure Storage tienen los siguientes requisitos adicionales:
  * El recurso de Azure Storage debe usar el tipo de cuenta StorageV2.
  * El recurso de Azure Cosmos DB debe usar MongoDB API.

Los escenarios de almacenamiento y las opciones de configuración asociadas se administran mediante el objeto `Storage`, en la sección de configuración `CloudAI`. Las opciones de configuración siguientes están disponibles en el objeto `Storage`:

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `StorageScenario` | string | Escenario de almacenamiento admitido por el contenedor. Están disponibles los valores siguientes:<br/>`Memory`: valor predeterminado. El contenedor usa un almacenamiento en memoria, no persistente y no distribuido para uso temporal y de nodo único. Si el contenedor se detiene o elimina, se destruye el almacenamiento de dicho contenedor.<br/>`Azure`: el contenedor usa recursos de Azure para el almacenamiento. Si el contenedor se detiene o elimina, se conserva el almacenamiento de dicho contenedor.|
| `ConnectionStringOfAzureStorage` | string | Cadena de conexión para el recurso de Azure Storage usado por el contenedor.<br/>Esta configuración solo se aplica si se especifica `Azure` para la opción de configuración `StorageScenario`. |
| `ConnectionStringOfCosmosMongo` | string | Cadena de conexión de MongoDB para el recurso de Azure Cosmos DB usado por el contenedor.<br/>Esta configuración solo se aplica si se especifica `Azure` para la opción de configuración `StorageScenario`. |

Por ejemplo, el siguiente comando especifica el escenario de almacenamiento de Azure y proporciona las cadenas de conexión de ejemplo para los recursos de Azure Storage y Cosmos DB usados para almacenar datos del contenedor de Face.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net" CloudAI:Storage:ConnectionStringOfAzureStorage="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
  ```

El escenario de almacenamiento se administra de forma independiente desde los montajes de entrada y salida. Puede especificar una combinación de estas características para un solo contenedor. Por ejemplo, el comando siguiente define un montaje de enlace de Docker para la carpeta `D:\Output` en el equipo host como montaje de salida y, a continuación, crea una instancia de un contenedora partir de la imagen de contenedor de Face, guardando los archivos de registro en formato JSON para el montaje de salida. El comando también especifica el escenario de almacenamiento de Azure y proporciona las cadenas de conexión de ejemplo para los recursos de Azure Storage y Cosmos DB usados para almacenar datos del contenedor de Face.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net" CloudAI:Storage:ConnectionStringOfAzureStorage="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
  ```

## <a name="eula-configuration-setting"></a>Opción de configuración Eula

La opción de configuración `Eula` indica que ha aceptado la licencia del contenedor. Debe especificar un valor para esta opción de configuración y el valor debe establecerse en `accept`.

> [!IMPORTANT]
> Las opciones de configuración [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) y [`Eula`](#eula-configuration-setting) se usan en conjunto, y debe proporcionar los valores válidos para las tres; en caso contrario, no se inicia el contenedor. Para obtener más información sobre el uso de estas opciones de configuración para crear instancias de un contenedor, consulte [Facturación](face-how-to-install-containers.md#billing).

## <a name="fluentd-configuration-settings"></a>Opciones de configuración Fluentd

La sección `Fluentd` administra las opciones de configuración de [Fluentd](https://www.fluentd.org), un recopilador de datos de código abierto para el registro unificado. El contenedor de Face incluye un proveedor de registro de Fluentd que permite que el contenedor escriba el registro y, opcionalmente, datos de métrica en un servidor de Fluentd.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `Fluentd`.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Host` | string | Dirección IP o nombre de host DNS del servidor de Fluentd. |
| `Port` | Entero | Puerto del servidor de Fluentd.<br/> El valor predeterminado es 24 224. |
| `HeartbeatMs` | Entero | Intervalo de latidos (en milisegundos). Si no se envía ningún tráfico de evento antes de que este intervalo expire, se envía un latido al servidor de Fluentd. El valor predeterminado es 60 000 milisegundos (1 minuto). |
| `SendBufferSize` | Entero | Espacio en búfer de red (en bytes) asignado para las operaciones de envío. El valor predeterminado es 32 768 bytes (32 kilobytes). |
| `TlsConnectionEstablishmentTimeoutMs` | Entero | Tiempo de expiración (en milisegundos) para establecer una conexión SSL/TLS con el servidor de Fluentd. El valor predeterminado es 10 000 milisegundos (10 segundos).<br/> Si `UseTLS` se establece en false, este valor se ignora. |
| `UseTLS` | boolean | Indica si el contenedor debe utilizar SSL/TLS para comunicarse con el servidor de Fluentd. El valor predeterminado es false. |

## <a name="logging-configuration-settings"></a>Opciones de configuración Logging

Las opciones de configuración `Logging` administran la compatibilidad con el registro de ASP.NET Core del contenedor. Puede usar los mismos valores y opciones de configuración para el contenedor que para una aplicación ASP.NET Core. Los siguientes proveedores de registro son compatibles con el contenedor de Face:

* [Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)  
  Proveedor de registro de `Console` de ASP.NET Core. Se admiten todos los valores predeterminados y las opciones de configuración de ASP.NET Core para este proveedor de registro.
* [Depuración](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)  
  Proveedor de registro de `Debug` de ASP.NET Core. Se admiten todos los valores predeterminados y las opciones de configuración de ASP.NET Core para este proveedor de registro.
* Disco  
  Proveedor de registro JSON. Este proveedor de registro escribe datos de registro para el montaje de salida.  
  El proveedor de registro `Disk` admite la configuración siguiente:  

  | NOMBRE | Tipo de datos | DESCRIPCIÓN |
  |------|-----------|-------------|
  | `Format` | string | Formato de salida de los archivos de registro.<br/> **Nota:** Este valor debe establecerse en `json` para habilitar el proveedor de registro. Si se especifica este valor sin especificar también un montaje de salida al crear una instancia de un contenedor, se produce un error. |
  | `MaxFileSize` | Entero | Tamaño máximo en megabytes (MB) de un archivo de registro. Cuando el tamaño del archivo de registro actual cumple este valor o lo supera, el proveedor de registro inicia un nuevo archivo de registro. Si se especifica -1, el tamaño del archivo de registro solo está limitado por el tamaño máximo de archivo, si existe, para el montaje de salida. El valor predeterminado es 1. |

Para obtener más información acerca de cómo configurar la compatibilidad con el registro de ASP.NET Core, consulte [Configuración del archivo de configuración](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mounts-configuration-settings"></a>Opciones de configuración Mounts

Los contenedores de Docker proporcionados por Face están diseñados para ser inmutables y sin estado. En otras palabras, los archivos creados dentro de un contenedor se almacenan en una capa grabable del contenedor, que solo se conserva mientras el contenedor se está ejecutando y a la que no se puede acceder fácilmente. Si el contenedor se detiene o elimina, los archivos creados dentro de él se destruyen.

Sin embargo, dado que son contenedores de Docker, puede usar las opciones de almacenamiento de Docker, como los montajes de enlace y volúmenes, para leer y escribir datos guardados fuera del contenedor, si lo admite el contenedor. Para obtener más información acerca de cómo especificar y administrar las opciones de almacenamiento de Docker, consulte [Manage data in Docker](https://docs.docker.com/storage/) (Administración de datos en Docker).

> [!NOTE]
> Normalmente, no tendrá que cambiar los valores de estas opciones de configuración. En su lugar, usará los valores especificados en estas opciones de configuración como destinos al especificar montajes de entrada y salida para el contenedor. Para obtener más información acerca de cómo especificar montajes de entrada y salida, consulte [Montajes de entrada y salida](#input-and-output-mounts).

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `Mounts`.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Input` | string | Destino del montaje de entrada. El valor predeterminado es `/input`. |
| `Output` | string | Destino del montaje de salida. El valor predeterminado es `/output`. |

### <a name="input-and-output-mounts"></a>Montajes de entrada y salida

De forma predeterminada, cada contenedor puede admitir un *montaje de entrada*, desde el que el contenedor puede leer datos, y un *montaje de salida*, en el que el contenedor puede escribir datos. No es necesario que los contenedores admitan montajes de entrada o salida, sin embargo, cada contenedor puede utilizar tanto montajes de entrada como de salida para fines específicos de los contenedores, además de las opciones de registro admitidas por el contenedor de Face.

El contenedor de Face no admite montajes de entrada y, opcionalmente, admite montajes de salida.

Puede especificar un montaje de entrada y uno de salida mediante la especificación de la opción `--mount` del comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) que se usa para crear una instancia de un contenedor a partir de una imagen de contenedor descargada. De forma predeterminada, el montaje de entrada usa `/input` como destino, mientras que el montaje de salida usa `/output`. Se puede especificar cualquier opción de almacenamiento de Docker disponible para el host de contenedor de Docker en la opción `--mount`.

Por ejemplo, el comando siguiente define un montaje de enlace de Docker para la carpeta `D:\Output` en el equipo host como montaje de salida y, a continuación, crea una instancia de un contenedora partir de la imagen de contenedor de Face, guardando los archivos de registro en formato JSON para el montaje de salida.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```

El contenedor de Face no usa los montajes de entrada o salida para almacenar datos de aprendizaje o de la base de datos. En su lugar, el contenedor de Face proporciona escenarios de almacenamiento para administrar los datos de aprendizaje y de la base de datos. Para obtener más información sobre el uso de escenarios de almacenamiento, consulte [Configuración de escenarios de almacenamiento](#storage-scenario-settings).