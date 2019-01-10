---
title: Creación de desencadenadores basados en eventos en Azure Data Factory | Microsoft Docs
description: Aprenda a crear un desencadenador en Azure Data Factory que ejecute una canalización en respuesta a un evento.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: douglasl
ms.openlocfilehash: 3fb9f98e94191c019b78c5666d2ff5336cc895eb
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54021873"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Creación de un desencadenador que ejecuta una canalización en respuesta a un evento

En este artículo se describen los desencadenadores basados en eventos que se pueden crear en las canalizaciones de Data Factory.

La arquitectura dirigida por eventos (EDA) es un patrón de integración de datos común que implica la producción, detección, consumo y reacción a los eventos. Los escenarios de integración de datos a menudo requieren clientes de Data Factory que desencadenen canalizaciones basadas en eventos. Data Factory está ahora integrado con [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), que le permite desencadenar canalizaciones en un evento.

Si desea ver una demostración y una introducción de diez minutos de esta característica, vea el siguiente vídeo de nueve minutos de duración:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> La integración descrita en este artículo depende de [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Asegúrese de que el proveedor de la suscripción se registra con el proveedor de recursos de Event Grid. Para más información, consulte [Tipos y proveedores de recursos](../azure-resource-manager/resource-manager-supported-services.md#portal).

## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory

### <a name="create-a-new-event-trigger"></a>Creación de un desencadenador de eventos

Un evento típico es la llegada de un archivo o la eliminación de un archivo, en la cuenta de Azure Storage. Puede crear un desencadenador que responda a este evento en la canalización de Data Factory.

> [!NOTE]
> Esta integración solo es compatible con las cuentas de Storage de la versión 2 (de uso general).

![Crear un desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="configure-the-event-trigger"></a>Configuración del desencadenador de eventos

Con las propiedades **Blob path begins with** y **Blob path ends with**, puede especificar los contenedores, carpetas y nombres de blob en los que desea recibir eventos. Como se muestra en ejemplos que podrá encontrar en este mismo artículo, se pueden usar varios patrones para las propiedades **Blob path begins with** y **Blob path ends with**. Al menos una de estas propiedades es obligatoria.

![Configuración del desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="select-the-event-trigger-type"></a>Selección del tipo de desencadenador de eventos

En cuanto el archivo llega a la ubicación de almacenamiento y se crea el blob correspondiente, se desencadena el evento y se ejecuta la canalización de Data Factory. En las canalizaciones de Data Factory puede crear un desencadenador que responda a un evento de creación de un blob, a un evento de eliminación de un blob, o a ambos eventos.

![Seleccionar un tipo de desencadenador como evento](media/how-to-create-event-trigger/event-based-trigger-image3.png)

### <a name="map-trigger-properties-to-pipeline-parameters"></a>Asignación de propiedades del desencadenador a parámetros de canalización

Cuando se activa un desencadenador de eventos para un blob concreto, el evento captura el nombre de archivo y la ruta de acceso de la carpeta del blob en las propiedades `@triggerBody().folderPath` y `@triggerBody().fileName`. Para usar los valores de estas propiedades en una canalización, debe asignar las propiedades a los parámetros de la canalización. Después de asignar las propiedades a los parámetros, puede tener acceso a los valores capturados por el desencadenador mediante la expresión `@pipeline().parameters.parameterName` en toda la canalización.

![Asignación de propiedades a los parámetros de la canalización](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Por ejemplo, en la captura de pantalla anterior, el desencadenador se configura para activarse cuando una ruta de acceso de blob que termine en `.csv` se crea en la cuenta de almacenamiento. Como resultado, cuando un blob con la extensión `.csv` se crea en algún lugar de la cuenta de almacenamiento, las propiedades `folderPath` y `fileName` capturan la ubicación del nuevo blob. Por ejemplo, `@triggerBody().folderPath` tiene un valor parecido a `/containername/foldername/nestedfoldername` y `@triggerBody().fileName` tiene un valor como `filename.csv`. Estos valores se asignan en el ejemplo a los parámetros de canalización `sourceFolder` y `sourceFile`. Puede usarlos en toda la canalización como `@pipeline().parameters.sourceFolder` y `@pipeline().parameters.sourceFile` respectivamente.

## <a name="json-schema"></a>Esquema JSON

En la tabla siguiente se proporciona información general acerca de los elementos de esquema que están relacionados con los desencadenadores basados en eventos:

| **Elemento de JSON** | **Descripción** | **Tipo** | **Valores permitidos** | **Obligatorio** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | El identificador de recursos de Azure Resource Manager de la cuenta de almacenamiento. | string | Identificador de Azure Resource Manager | SÍ |
| **eventos** | El tipo de eventos que provocan la activación de este desencadenador. | Matriz    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Sí, cualquier combinación de estos valores. |
| **blobPathBeginsWith** | La ruta de acceso del blob debe comenzar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, `/records/blobs/december/` solo activa el desencadenador de blobs en la carpeta `december` bajo el contenedor `records`. | string   | | Tendrá que proporcionar un valor para al menos una de estas propiedades: `blobPathBeginsWith` o `blobPathEndsWith`. |
| **blobPathEndsWith** | La ruta de acceso del blob debe finalizar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, `december/boxes.csv` solo activa el desencadenador de blobs denominado "`boxes`" en una carpeta `december`. | string   | | Tendrá que proporcionar un valor para al menos una de estas propiedades: `blobPathBeginsWith` o `blobPathEndsWith`. |

## <a name="examples-of-event-based-triggers"></a>Ejemplos de desencadenadores basados en eventos

En esta sección encontrará ejemplos de configuración de desencadenadores basados en eventos.

> [!IMPORTANT]
> Debe incluir el segmento `/blobs/` de la ruta de acceso, tal como se muestra en los siguientes ejemplos, siempre que especifique el contenedor y la carpeta, el contenedor y el archivo, o el contenedor, la carpeta y el archivo.

| Propiedad | Ejemplo | DESCRIPCIÓN |
|---|---|---|
| **Ruta de acceso de blobs que empieza con** | `/containername/` | Recibe eventos de cualquier blob del contenedor. |
| **Ruta de acceso de blobs que empieza con** | `/containername/blobs/foldername/` | Recibe eventos de los blobs en el contenedor `containername` y la carpeta `foldername`. |
| **Ruta de acceso de blobs que empieza con** | `/containername/blobs/foldername/subfoldername/` | También puede hacer referencia a una subcarpeta. |
| **Ruta de acceso de blobs que empieza con** | `/containername/blobs/foldername/file.txt` | Recibe eventos de un blob denominado `file.txt` en la carpeta `foldername`, en el contenedor `containername`. |
| **Ruta de acceso de blobs que termina con** | `file.txt` | Recibe eventos de un blob denominado `file.txt` en cualquier ruta de acceso. |
| **Ruta de acceso de blobs que termina con** | `/containername/blobs/file.txt` | Recibe eventos de un blob denominado `file.txt` en el contenedor `containername`. |
| **Ruta de acceso de blobs que termina con** | `foldername/file.txt` | Recibe eventos de un blob denominado `file.txt` en la carpeta `foldername` en cualquier contenedor. |

## <a name="next-steps"></a>Pasos siguientes
Para obtener información detallada acerca de los desencadenadores, consulte el artículo [Pipeline execution and triggers](concepts-pipeline-execution-triggers.md#triggers) (Ejecución de canalizaciones y desencadenadores).
