---
title: 'Almacenamiento de blob en bloques en los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Comprender las características de organización en niveles y período de vida, vea las operaciones de almacenamiento de blobs compatibles y conectarse a la cuenta de almacenamiento de blobs.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 396af2dfd9fc53c080163a27e376328c1369d5e1
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991464"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Almacene datos en el perímetro con Azure Blob Storage en IoT Edge (versión preliminar)

Azure Blob Storage en IoT Edge proporciona una solución de almacenamiento de [blobs en bloques](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) en el perímetro. En los dispositivos con IoT Edge, los módulos de almacenamiento de blobs se comportan como un servicio de blobs en bloques de Azure, pero los blobs en bloques se almacenan localmente en el dispositivo IoT Edge. Para acceder a los blobs puede usar los mismos métodos que el SDK de Azure Storage o las llamadas API del blob en bloques a las que está acostumbrado.

> [!NOTE]
> Azure Blob Storage en IoT Edge está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Este módulo incluye **niveles** y **time-to-live** características.

> [!NOTE]
> Período de vida de la funcionalidad y las capas actualmente solo están disponibles en Linux AMD64 y Linux ARM32.

**Los niveles** es una funcionalidad configurable, que le permite cargar automáticamente los datos de su almacenamiento de blobs local a Azure con compatibilidad con conectividad de internet intermitentes. Le permite:

- Encendido de activar la característica de niveles
- Elija el orden en que los datos se copian en Azure como NewestFirst o OldestFirst
- Especifique la cuenta de Azure Storage al que desea que los datos cargados.
- Especifique el contenedor que desee cargar en Azure. Este módulo le permite especificar los nombres de contenedor de origen y destino.
- Total de los niveles de blob (mediante `Put Blob` operación) y bloquear el nivel de organización en niveles (mediante `Put Block` y `Put Block List` operaciones).

Este módulo utiliza el bloque niveles, cuando el blob se compone de bloques. Estos son algunos de los escenarios comunes:

- La aplicación actualiza algunos bloques de un blob cargado anteriormente, este módulo de carga solo los bloques actualizados y no la totalidad del blob.
- El módulo carga el blob y conexión a internet desaparece, cuando la conectividad vuelve atrás que se cargan solo los bloques restantes y no la totalidad del blob.

Si se produce una finalización inesperada de proceso (por ejemplo, un corte de suministro eléctrico) durante una carga de blobs, todos los bloques que vencieron para la carga se cargará de nuevo, cuando vuelve a conectar el módulo.

**Período de vida** (TTL) es una funcionalidad configurable donde el módulo elimina automáticamente los blobs desde el almacenamiento local cuando expira el tiempo especificado (medido en minutos). TTL le permite:

- Encendido de activar la característica de niveles
- Especifique el valor de TTL en minutos

Escenarios donde los datos, como vídeos, imágenes, datos financieros, datos de hospital o los datos que tengan que almacenarse localmente, más adelante que podían procesarse de forma local o transferidos a la nube son buenos ejemplos para usar este módulo.

En este artículo se proporcionan instrucciones para la implementación de Azure Blob Storage en un contenedor de IoT Edge en el que se ejecuta un servicio de blobs en un dispositivo de IoT Edge.

> [!NOTE]
> Los términos de "auto-niveles" y "auto-expiración" usada en el vídeo se han reemplazado por "niveles" y "time-to-live".

Ver el vídeo de introducción rápida
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

- Puede usar el equipo de desarrollo o una máquina virtual como un dispositivo IoT Edge siguiendo los pasos descritos en la Guía de inicio rápido para [Linux](quickstart-linux.md) o [dispositivos Windows](quickstart.md).

- El módulo de Azure Blob Storage en IoT Edge admite las siguientes configuraciones de dispositivo:

  | Sistema operativo | Arquitectura |
  | ---------------- | ----- | ----- |
  | Ubuntu Server 16.04 | AMD64 |
  | Ubuntu Server 18.04 | AMD64 |
  | IoT Core para Windows 10 (actualización de octubre) | AMD64 |
  | IoT Enterprise para Windows 10 (actualización de octubre) | AMD64 |
  | Windows Server 2019 | AMD64 |
  | Raspbian-stretch | ARM32 |

Recursos en la nube:

Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar en Azure.

## <a name="tiering-and-time-to-live-properties"></a>Propiedades de los niveles y período de vida

Utilice las propiedades deseadas para establecer los niveles y las propiedades de período de vida. Puede establecer durante la implementación o cambiar más adelante mediante la edición del módulo gemelo sin necesidad de volver a implementar. Se recomienda comprobar el "módulo gemelo" para `reported configuration` y `configurationValidation` para asegurarse de que los valores se propagan correctamente.

### <a name="tiering-properties"></a>Propiedades de la organización en niveles

El nombre de esta configuración es `tieringSettings`

| Campo | Valores posibles | Explicación |
| ----- | ----- | ---- |
| tieringOn | true, false | De forma predeterminada se establece en `false`, si desea convertirlo en establézcalo en `true`|
| backlogPolicy | NewestFirst, OldestFirst | Le permite elegir el orden en que los datos se copian en Azure. De forma predeterminada se establece en `OldestFirst`. El orden viene determinada por la hora última modificación del Blob |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` ¿se carga una cadena de conexión que le permite especificar la cuenta de almacenamiento de Azure a la que desea que los datos. Especificar `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Agregar adecuado EndpointSuffix de Azure donde se cargarán los datos, varía para Global Azure, Azure Government y Microsoft Azure Stack. |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Le permite especificar los nombres de contenedor que desea cargar en Azure. Este módulo le permite especificar los nombres de contenedor de origen y destino. Si no especifica el nombre del contenedor de destino, asignará automáticamente el nombre del contenedor como `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. Puede crear cadenas de plantillas para el nombre del contenedor de destino, consulte la columna de valores posibles. <br>* %h -> nombre de IoT Hub (entre 3 y 50 caracteres). <br>* %d -> Id. de dispositivo de IoT (1 a 129 caracteres). <br>* %m -> nombre del módulo (1 a 64 caracteres). <br>* %c -> nombre del contenedor de origen (3 y 63 caracteres). <br><br>Tamaño máximo del nombre del contenedor es de 63 caracteres, al asignar automáticamente el nombre del contenedor de destino si el tamaño del contenedor es superior a 63 caracteres, que eliminará cada sección (IoTHubName, IotEdgeDeviceName, ModuleName, ContainerName) y 15 caracteres. |

### <a name="time-to-live-properties"></a>Propiedades del período de vida

El nombre de esta configuración es `ttlSettings`

| Campo | Valores posibles | Explicación |
| ----- | ----- | ---- |
| ttlOn | true, false | De forma predeterminada se establece en `false`, si desea convertirlo en establézcalo en `true`|
| timeToLiveInMinutes | `<minutes>` | Especifique el valor de TTL en minutos. El módulo eliminará automáticamente los blobs de almacenamiento local cuando expira el período de vida |

## <a name="configure-log-files"></a>Configurar archivos de registro

Para obtener información sobre cómo configurar los archivos de registro para el módulo, consulte estos [prácticas recomendadas de producción](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics).

## <a name="connect-to-your-blob-storage-module"></a>Conexión a un módulo de Blob Storage

Puede usar el nombre y la clave de cuenta que configuró para el módulo para acceder al almacenamiento de blobs en el dispositivo de IoT Edge.

Especifique el dispositivo de IoT Edge como el punto de conexión del blob para todas las solicitudes de almacenamiento que realice en él. Puede [crear una cadena de conexión para un punto de conexión de almacenamiento explícito](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) mediante la información del dispositivo de IoT Edge y el nombre de cuenta que configuró.

- Para los módulos que se implementan en el mismo dispositivo como que se ejecuta en el módulo de IoT Edge en Azure Blob Storage, es el punto de conexión de blob: `http://<module name>:11002/<account name>`.
- Los módulos que se implementan en un dispositivo diferente donde se está ejecutando el almacenamiento de blobs de Azure en el módulo de IoT Edge, entonces según la configuración del blob de punto de conexión es uno de:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Ejemplos de inicio rápido de Azure Blob Storage

La documentación de Azure Blob Storage incluye guías de inicio rápido que proporcionan ejemplo de código en varios idiomas. Puede ejecutar estos ejemplos para probar Azure Blob Storage en IoT Edge cambiando el punto de conexión de blob para conectarse a su módulo de almacenamiento de blobs.

Las siguientes guías de inicio rápido usan lenguajes que también son compatibles con IoT Edge, por lo que pueden implementarlas como módulos de IoT Edge, junto con el módulo de Blob Storage:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Conectarse a su almacenamiento local con el Explorador de Storage de Azure

Puede usar **Explorador de Azure Storage** para conectarse a su cuenta de almacenamiento local. Esto solo está disponible con [Explorador de Azure Storage versión 1.5.0](https://github.com/Microsoft/AzureStorageExplorer/releases/tag/v1.5.0).

> [!NOTE]
> Pueden producirse errores al realizar los pasos siguientes, como agregar una conexión a una cuenta de almacenamiento local, o la creación de contenedores en la cuenta de almacenamiento local. Omítalo y actualizar.

1. Descargue e instale el Explorador de Azure Storage

1. Conectarse a Azure Storage mediante una cadena de conexión

1. Proporcionar la cadena de conexión: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Siga los pasos para conectarse.

1. Crear contenedor dentro de su cuenta de almacenamiento local

1. Iniciar la carga de archivos como blobs en bloques.
   > [!NOTE]
   > Este módulo no es compatible con blobs en páginas.

1. Puede elegir conectarse a cuentas de Azure storage donde va a cargar los datos. Proporciona una vista única para la cuenta de almacenamiento local y la cuenta de almacenamiento de Azure

## <a name="supported-storage-operations"></a>Operaciones de Blob Storage

Los módulos de almacenamiento de blobs en IoT Edge usan el mismo SDK de almacenamiento de Azure y son coherentes con la versión 2017-04-17 de la API de Azure Storage para puntos de conexión de blob de bloque. Las versiones posteriores dependen de las necesidades del cliente.

Dado que no todas las operaciones de almacenamiento de blobs de Azure son compatibles con Azure Blob Storage en IoT Edge, esta sección enumera el estado de cada uno.

### <a name="account"></a>Cuenta

Compatible:

- Enumerar contenedores

No compatible:

- Obtener y definir las propiedades del servicio Blob
- Preparar solicitud de blob
- Obtener estadísticas del servicio Blob
- Obtención de información de la cuenta

### <a name="containers"></a>Contenedores

Compatible:

- Crear y eliminar contenedor
- Obtener de propiedades y metadatos del contenedor
- Mostrar blobs
- Obtener y definir lista de control de acceso de contenedor
- Establecer metadatos de contenedor

No compatible:

- Conceder contenedor

### <a name="blobs"></a>Blobs

Compatible:

- Poner, obtener y eliminar blob
- Obtener y definir propiedades de blob
- Obtener y definir metadatos de blob

No compatible:

- Conceder blob
- Instantánea de blob
- Copiar y anular blob de copia
- Recuperar blob
- Establecer nivel de blob

### <a name="block-blobs"></a>Blobs en bloques

Compatible:

- Colocar bloque
- Colocar y obtener lista de bloques

No compatible:

- Colocar bloque desde dirección URL

## <a name="feedback"></a>Comentarios

Sus comentarios son importantes para nosotros a fin de que este módulo y sus características, útil y fácil de usar. Comparta sus comentarios y háganos saber cómo podemos mejorar.

Puede ponerse en contacto nosotros en absiotfeedback@microsoft.com

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [implementar Azure Blob Storage en IoT Edge](how-to-deploy-blob.md)
