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
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: douglasl
ms.openlocfilehash: 457983021034d83e0eed05bd91eae1ac30c046da
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296882"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Creación de un desencadenador que ejecuta una canalización en respuesta a un evento

En este artículo se describen los desencadenadores basados en eventos que se pueden crear en las canalizaciones de Data Factory.

La arquitectura dirigida por eventos (EDA) es un patrón de integración de datos común que implica la producción, detección, consumo y reacción a los eventos. Los escenarios de integración de datos a menudo requieren clientes de Data Factory que desencadenen canalizaciones basadas en eventos.

## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory

### <a name="create-a-new-event-trigger"></a>Creación de un desencadenador de eventos

Un evento típico es la llegada de un archivo o la eliminación de un archivo, en la cuenta de Azure Storage. Puede crear un desencadenador que responda a este evento en la canalización de Data Factory.

![Crear un desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="select-the-event-trigger-type"></a>Selección del tipo de desencadenador de eventos

En cuanto el archivo llega a la ubicación de almacenamiento y se crea el blob correspondiente, se desencadena el evento y se ejecuta la canalización de Data Factory. En las canalizaciones de Data Factory puede crear un desencadenador que responda a un evento de creación de un blob, a un evento de eliminación de un blob, o a ambos eventos.

![Seleccionar un tipo de desencadenador como evento](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="configure-the-event-trigger"></a>Configuración del desencadenador de eventos

Con las propiedades **Blob path begins with** y **Blob path ends with**, puede especificar los contenedores, carpetas y nombres de blob en los que desea recibir eventos. Como se muestra en ejemplos que podrá encontrar en este mismo artículo, se pueden usar varios patrones para las propiedades **Blob path begins with** y **Blob path ends with**. Al menos una de estas propiedades es obligatoria.

![Configuración del desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image3.png)

## <a name="json-schema"></a>Esquema JSON

En la tabla siguiente se proporciona información general acerca de los elementos de esquema que están relacionados con los desencadenadores basados en eventos:

| **Elemento de JSON** | **Descripción** | **Tipo** | **Valores permitidos** | **Obligatorio** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | El identificador de recursos de Azure Resource Manager de la cuenta de almacenamiento. | string | Identificador de Azure Resource Manager | Sí |
| **eventos** | El tipo de eventos que provocan la activación de este desencadenador. | Matriz    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Sí, cualquier combinación. |
| **blobPathBeginsWith** | La ruta de acceso del blob debe comenzar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, "/records/blobs/december/" solo activará el desencadenador de los blobs de la carpeta december del contenedor records. | string   | | Se debe proporcionar al menos una de estas propiedades: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | La ruta de acceso del blob debe finalizar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, "december/boxes.csv" solo activará el desencadenador de blobs llamado boxes de la carpeta december. | string   | | Se debe proporcionar al menos una de estas propiedades: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Ejemplos de desencadenadores basados en eventos

En esta sección encontrará ejemplos de configuración de desencadenadores basados en eventos.

-   **Blob path begins with**('/NombreDeContenedor/'): recibe eventos de cualquier blob del contenedor.
-   **Blob path begins with**('/NombreDeContenedor/NombreDeCarpeta'): recibe eventos de los blobs del contenedor NombreDeContenedor de la carpeta NombreDeCarpeta.
-   **Blob path begins with**('/NombreDeContenedor/NombreDeCarpeta/Archivo.txt') – recibe eventos de un blob llamado Archivo.txt de la carpeta NombreDeCarpeta del contenedor NombreDeContenedor.
-   **Blob path ends with**('Archivo.txt'): recibe eventos de un blob llamado Archivo.txt en cualquier ruta de acceso.
-   **Blob path ends with**('/NombreDeContenedor/Archivo.txt'): recibe eventos de un blob llamado Archivo.txt del contenedor NombreDeContenedor.
-   **Blob path ends with**('NombreDeCarpeta/Archivo.txt') – recibe eventos de un blob llamado Archivo.txt de la carpeta NombreDeCarpeta de cualquier contenedor.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información detallada acerca de los desencadenadores, consulte el artículo [Pipeline execution and triggers](concepts-pipeline-execution-triggers.md#triggers) (Ejecución de canalizaciones y desencadenadores).
