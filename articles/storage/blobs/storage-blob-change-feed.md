---
title: Fuente de cambios en Azure Blob Storage (versión preliminar) | Microsoft Docs
description: Obtenga información sobre los registros de fuente de cambios en Azure Blob Storage y cómo usarlos.
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: 07123fd5701e9041ff377ea5309cf1291e737ca6
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73693813"
---
# <a name="change-feed-support-in-azure-blob-storage-preview"></a>Compatibilidad con la fuente de cambios en Azure Blob Storage (versión preliminar)

El propósito de la fuente de cambios es proporcionar registros de transacciones de todos los cambios que se producen en los blobs y en los metadatos de blobs de la cuenta de almacenamiento. La fuente de cambios proporciona un registro **ordenado**, **garantizado**, **durable**, **inmutable** y de **solo lectura** de estos cambios. Las aplicaciones cliente pueden leer estos registros en cualquier momento, ya sea en streaming o en modo por lotes. La fuente de cambios le permite compilar soluciones eficaces y escalables que procesan los eventos de cambio que se producen en su cuenta de Blob Storage a un bajo costo.

> [!NOTE]
> La fuente de cambios está en versión preliminar pública y se encuentra disponible en las regiones **westcentralus** y **westus2**. Consulte la sección de [condiciones](#conditions) de este artículo. Para inscribirse en la versión preliminar, consulte la sección [Registro de la suscripción](#register) de este artículo.

La fuente de cambios se almacena como [blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) en un contenedor especial de la cuenta de almacenamiento al costo de los [precios de los blobs](https://azure.microsoft.com/pricing/details/storage/blobs/) estándar. Puede controlar el período de retención de estos archivos en función de los requisitos (consulte las [condiciones](#conditions) de la versión actual). Los eventos de cambio se anexan a la fuente de cambios como registros en la especificación de formato de [Apache Avro](https://avro.apache.org/docs/1.8.2/spec.html): un formato compacto, rápido y binario que proporciona estructuras de datos enriquecidos con el esquema en línea. Este formato se usa ampliamente en el ecosistema de Hadoop, en Stream Analytics y en Azure Data Factory.

Puede procesar estos registros de manera asincrónica, incremental o en su totalidad. Cualquier número de aplicaciones cliente puede leer de manera independiente la fuente de cambios, en paralelo y a su propio ritmo. Las aplicaciones de análisis como [Apache Drill](https://drill.apache.org/docs/querying-avro-files/) o [Apache Spark](https://spark.apache.org/docs/latest/sql-data-sources-avro.html) pueden consumir registros directamente como archivos Avro, lo que le permite procesarlos a un bajo costo, con un alto ancho de banda y sin la necesidad de escribir una aplicación personalizada.

La compatibilidad con la fuente de cambios es adecuada para escenarios que procesan datos en función de los objetos que han cambiado. Por ejemplo, las aplicaciones pueden:

  - Actualizar un índice secundario, sincronizar con una caché, un motor de búsqueda o cualquier otro escenario de administración de contenido.
  
  - Extraer métricas e información de análisis de negocios, en función de los cambios que se produzcan en los objetos, ya sea como transmisión o en modo por lotes.
  
  - Almacenar, auditar y analizar cambios en los objetos, en cualquier período de tiempo, por seguridad, cumplimiento normativo o inteligencia de la administración de datos empresariales.

  - Compilar soluciones para la copia de seguridad, el reflejo o la replicación del estado de los objetos en su cuenta para la administración ante desastres o el cumplimiento.

  - Crear canalizaciones de aplicaciones conectadas que reaccionen a eventos de cambio o programen ejecuciones basadas en objetos creados o modificados.

> [!NOTE]
> Los [eventos de Blob Storage](storage-blob-event-overview.md) proporcionan eventos únicos en tiempo real que permiten Azure Functions o sus aplicaciones reaccionen a los cambios que se producen en un blob. La fuente de cambios proporciona un modelo de registro duradero y ordenado de los cambios. Los cambios en la fuente de cambios se ponen a disposición en la fuente de cambios en cuestión de minutos después del cambio. Si es necesario que su aplicación reaccione a eventos mucho más rápido, considere la posibilidad de usar en su lugar los [eventos de Blob Storage](storage-blob-event-overview.md). Los eventos de Blob Storage permiten que Azure Functions o las aplicaciones reaccionen ante eventos individuales en tiempo real.

## <a name="enabling-and-disabling-the-change-feed"></a>Habilitación y deshabilitación de la fuente de cambios

Para iniciar la captura de cambios, es necesario habilitar la fuente de cambios. Deshabilite la fuente de cambios para detener la captura de cambios. Puede habilitar y deshabilitar los cambios mediante el uso de plantillas de Azure Resource Manager en el portal o PowerShell.

### <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Para implementar la plantilla con Azure Portal:

1. En Azure Portal, elija **Crear un recurso**.

2. En **Buscar en Marketplace**, escriba **implementación de plantillas** y, después, presione **ENTRAR**.

3. Elija **Template Deployment**, elija **Crear** y, luego, **Cree su propia plantilla en el editor**.

5. En el editor de plantillas, pegue el JSON siguiente. Reemplace el marcador de posición `<accountName>` por el nombre de la cuenta de almacenamiento.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [{
        "type": "Microsoft.Storage/storageAccounts/blobServices",
        "apiVersion": "2019-04-01",
        "name": "<accountName>/default",
        "properties": {
            "changeFeed": {
            "enabled": true
            }
        } 
     }]
}
```
4. Elija el botón **Guardar**, especifique el grupo de recursos de la cuenta y, luego, elija el botón **Comprar** para habilitar la fuente de cambios.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Para implementar la plantilla con PowerShell:

1. Instale el PowershellGet más reciente.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force
   ```

2. Cierre la consola de PowerShell y vuelva a abrirla.

3. Instale el módulo de versión preliminar **Az.Storage**.

   ```powershell
   Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.8.1-preview –AllowPrerelease –AllowClobber –Force
   ```

4. Inicie sesión en la suscripción de Azure con el comando `Connect-AzAccount` y siga las instrucciones que aparecen en pantalla para autenticarse.

   ```powershell
   Connect-AzAccount
   ```

5. Habilite la fuente de cambios para la cuenta de almacenamiento.

   ```powershell
   Update-AzStorageBlobServiceProperty -ResourceGroupName -StorageAccountName -EnableChangeFeed $true
   ```

---

Estos son algunos aspectos que hay que tener en cuenta al habilitar la fuente de cambios.

- Solo hay una fuente de cambios para Blob service en cada cuenta de almacenamiento. 

- Los cambios se capturan solo en el nivel de Blob service.

- La fuente de cambios captura *todos* los cambios de todos los eventos disponibles que se producen en la cuenta. Las aplicaciones cliente pueden filtrar los tipos de eventos según sea necesario. (Consulte las [condiciones](#conditions) de la versión actual).

- No se admiten las cuentas con un espacio de nombres jerárquico.

## <a name="consuming-the-change-feed"></a>Consumo de la fuente de cambios

La fuente de cambios genera varios archivos de registro y metadatos. Estos archivos se ubican en el contenedor **$blobchangefeed** de la cuenta de almacenamiento. 

>[!NOTE]
> En la versión actual, el contenedor **$blobchangefeed** no está visible en el Explorador de Storage ni en Azure Portal. 

Las aplicaciones cliente pueden usar la fuente de cambios mediante la biblioteca de procesadores de la fuente de cambios de blob que se proporciona con el SDK. 

Consulte [Procesamiento de los registros de la fuente de cambios en Azure Blob Storage](storage-blob-change-feed-how-to.md).

## <a name="understanding-change-feed-organization"></a>Descripción de la organización de la fuente de cambios

<a id="segment-index"></a>

### <a name="segments"></a>Segmentos

La fuente de cambios es un registro de cambios que se organiza en *segmentos* **cada hora** (consulte las [especificaciones](#specifications)). Esto permite que la aplicación cliente consuma los cambios que se producen dentro de intervalos de tiempo específicos sin tener que buscar en todo el registro.

Un segmento por hora disponible de la fuente de cambios se describe en un archivo de manifiesto que especifica las rutas de acceso a los archivos de la fuente de cambios para ese segmento. El listado del directorio virtual `$blobchangefeed/idx/segments/` muestra estos segmentos ordenados por hora. La ruta de acceso del segmento describe el inicio del intervalo de tiempo por hora que el segmento representa. (Consulte las [especificaciones](#specifications)). Puede usar esa lista para filtrar los segmentos de registros que le interesan.

```text
Name                                                                    Blob Type    Blob Tier      Length  Content Type    
----------------------------------------------------------------------  -----------  -----------  --------  ----------------
$blobchangefeed/idx/segments/1601/01/01/0000/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/22/1810/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/22/1910/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/23/0110/meta.json                  BlockBlob                      584  application/json
```

> [!NOTE]
> `$blobchangefeed/idx/segments/1601/01/01/0000/meta.json` se crea automáticamente cuando se habilita la fuente de cambios. Puede omitir este archivo sin problemas. Siempre está vacío. 

El archivo de manifiesto del segmento (`meta.json`) muestra la ruta de acceso de los archivos de la fuente de cambios para ese segmento en la propiedad `chunkFilePaths`. A continuación, se muestra un ejemplo de un archivo de manifiesto de segmento.

```json
{
    "version": 0,
    "begin": "2019-02-22T18:10:00.000Z",
    "intervalSecs": 3600,
    "status": "Finalized",
    "config": {
        "version": 0,
        "configVersionEtag": "0x8d698f0fba563db",
        "numShards": 2,
        "recordsFormat": "avro",
        "formatSchemaVersion": 1,
        "shardDistFnVersion": 1
    },
    "chunkFilePaths": [
        "$blobchangefeed/log/00/2019/02/22/1810/",
        "$blobchangefeed/log/01/2019/02/22/1810/"
    ],
    "storageDiagnostics": {
        "version": 0,
        "lastModifiedTime": "2019-02-22T18:11:01.187Z",
        "data": {
            "aid": "55e507bf-8006-0000-00d9-ca346706b70c"
        }
    }
}
```

> [!NOTE]
> Si enumera los contenedores de la cuenta de almacenamiento, el contenedor `$blobchangefeed` solo aparece después de haber habilitado la característica de fuente de cambios en su cuenta. Tendrá que esperar unos minutos después de habilitar la fuente de cambios para poder ver el contenedor.

<a id="log-files"></a>

### <a name="change-event-records"></a>Registros de eventos de cambio

Los archivos de la fuente de cambios contienen una serie de registros de eventos de cambio. Cada registro de evento de cambio corresponde a un cambio en un blob individual. Los registros se serializan y se escriben en el archivo mediante la especificación de formato de [Apache Avro](https://avro.apache.org/docs/1.8.2/spec.html). Los registros se pueden leer si se usa la especificación de formato de archivo Avro. Hay varias bibliotecas disponibles para procesar archivos en ese formato.

Los archivos de la fuente de cambios se almacenan en el directorio virtual `$blobchangefeed/log/` como [blobs en anexos](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs). El primer archivo de fuente de cambios de cada ruta de acceso tendrá `00000` en el nombre de archivo (por ejemplo, `00000.avro`). El nombre de cada archivo de registro subsiguiente agregado a esa ruta de acceso se incrementará en 1 (por ejemplo, `00001.avro`).

Este es un ejemplo de registro de evento de cambio del archivo de la fuente de cambios convertido en JSON.

```json
{
     "schemaVersion": 1,
     "topic": "/subscriptions/dd40261b-437d-43d0-86cf-ef222b78fd15/resourceGroups/sadodd/providers/Microsoft.Storage/storageAccounts/mytestaccount",
     "subject": "/blobServices/default/containers/mytestcontainer/blobs/mytestblob",
     "eventType": "BlobCreated",
     "eventTime": "2019-02-22T18:12:01.079Z",
     "id": "55e5531f-8006-0000-00da-ca3467000000",
     "data": {
         "api": "PutBlob",
         "clientRequestId": "edf598f4-e501-4750-a3ba-9752bb22df39",
         "requestId": "00000000-0000-0000-0000-000000000000",
         "etag": "0x8D698F13DCB47F6",
         "contentType": "application/octet-stream",
         "contentLength": 128,
         "blobType": "BlockBlob",
         "url": "",
         "sequencer": "000000000000000100000000000000060000000000006d8a",
         "storageDiagnostics": {
             "bid": "11cda41c-13d8-49c9-b7b6-bc55c41b3e75",
             "seq": "(6,5614,28042,28038)",
             "sid": "591651bd-8eb3-c864-1001-fcd187be3efd"
         }
  }
}

```
Para una descripción de cada propiedad, consulte [Esquema de eventos de Azure Event Grid para Blob Storage](https://docs.microsoft.com/azure/event-grid/event-schema-blob-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#event-properties).

> [!NOTE]
> Los archivos de la fuente de cambios de un segmento no aparecen inmediatamente después de crear un segmento. La duración del retraso se encuentra dentro del intervalo normal de la latencia de publicación de la fuente de cambios que se encuentra dentro de unos minutos del cambio.

<a id="specifications"></a>

## <a name="specifications"></a>Especificaciones

- Los registros de eventos de cambio solo se anexan a la fuente de cambios. Una vez que se anexan estos registros, son inmutables y la posición de registro es estable. Las aplicaciones cliente pueden mantener su propio punto de control en la posición de lectura de la fuente de cambios.

- Los registros de eventos de cambio se anexan en cuestión de minutos después del cambio. Las aplicaciones cliente pueden optar por consumir los registros a medida que se anexan para el acceso de streaming o de manera masiva en cualquier otro momento.

- Los registros de eventos de cambio se organizan por orden de modificación **por blob**. El orden de los cambios entre blobs no está definido en Azure Blob Storage. Todos los cambios en un segmento anterior son previos a cualquier cambio en los segmentos posteriores.

- Los registros de eventos de cambio se serializan en el archivo de registro mediante la especificación de formato [Apache Avro 1.8.2](https://avro.apache.org/docs/1.8.2/spec.html).

- Los registros de eventos de cambio en los que `eventType` tiene un valor de `Control` son registros del sistema interno y no reflejan un cambio en los objetos de la cuenta. Debe omitirlos.

- Los valores del contenedor de propiedades `storageDiagnonstics` son solo para uso interno y no están diseñados para su uso por parte de la aplicación. Las aplicaciones no deben tener una dependencia contractual de esos datos.

- El tiempo representado por el segmento es **aproximado** con límites de 15 minutos. Por lo tanto, para garantizar el consumo de todos los registros dentro de un tiempo especificado, consuma el segmento de hora consecutivo anterior y siguiente.

- Cada segmento puede tener un número distinto de `chunkFilePaths`. Esto se debe a la creación de particiones internas de la secuencia de registro para administrar el rendimiento de la publicación. Se garantiza que los archivos de registro de cada `chunkFilePath` contienen blobs mutuamente excluyentes y se pueden consumir y procesar en paralelo sin infringir el orden de las modificaciones por blob durante la iteración.

- Los segmentos empiezan con el estado `Publishing`. Una vez que los registros se anexen al segmento, el estado será `Finalized`. La aplicación no debe utilizar los archivos de registro de un segmento que tenga una fecha posterior a la fecha de la propiedad `LastConsumable` en el archivo `$blobchangefeed/meta/Segments.json`. Este es un ejemplo de la propiedad `LastConsumable` en un archivo `$blobchangefeed/meta/Segments.json`:

```json
{
    "version": 0,
    "lastConsumable": "2019-02-23T01:10:00.000Z",
    "storageDiagnostics": {
        "version": 0,
        "lastModifiedTime": "2019-02-23T02:24:00.556Z",
        "data": {
            "aid": "55e551e3-8006-0000-00da-ca346706bfe4",
            "lfz": "2019-02-22T19:10:00.000Z"
        }
    }
}

```

<a id="register"></a>

## <a name="register-your-subscription-preview"></a>Registro de la suscripción (versión preliminar)

Dado que la fuente de cambios solo está en versión preliminar pública, deberá registrar su suscripción para usar la característica.

### <a name="register-by-using-powershell"></a>Registro mediante PowerShell

En una consola de PowerShell, ejecute estos comandos:

   ```powershell
   Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
   ```
### <a name="register-by-using-azure-cli"></a>Registro mediante la CLI de Azure

En Azure Cloud Shell, ejecute estos comandos:

```cli
az feature register --namespace Microsoft.Storage --name Changefeed
az provider register --namespace 'Microsoft.Storage'
```

<a id="conditions"></a>

## <a name="conditions-and-known-issues-preview"></a>Condiciones y problemas conocidos (versión preliminar)

En esta sección se describen los problemas conocidos y las condiciones de la versión preliminar pública actual de la fuente de cambios.

- La fuente de cambios solo captura las operaciones de creación, actualización, eliminación y copia.
- Los registros de eventos de cambio para cualquier cambio único pueden aparecer más de una vez en la fuente de cambios.
- Todavía no se puede administrar la duración de los archivos de registro de la fuente de cambios al establecer en ellos la directiva de retención basada en tiempo.
- La propiedad `url` del archivo de registro siempre está vacía.
- La propiedad `LastConsumable` del archivo segment.json no muestra el primer segmento que la fuente de cambios finaliza. Este problema solo se produce una vez finalizado el primer segmento. Todos los segmentos posteriores después de la primera hora se capturan con precisión en la propiedad `LastConsumable`.

## <a name="next-steps"></a>Pasos siguientes

- Consulte un ejemplo de cómo leer la fuente de cambios mediante una aplicación cliente de .NET. Consulte [Procesamiento de los registros de la fuente de cambios en Azure Blob Storage](storage-blob-change-feed-how-to.md).
- Aprenda a reaccionar ante eventos en tiempo real. Consulte [Reacción ante eventos de Blob Storage](storage-blob-event-overview.md).
