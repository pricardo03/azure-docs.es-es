---
title: Adición de un enlace de cola de Azure Storage a la función de Python
description: Aprenda a agregar un enlace de salida de cola de Azure Storage a la función de Python.
ms.date: 10/02/2019
ms.topic: quickstart
ms.openlocfilehash: f2db0f470c4205919343e3838a4f73b05bf71fb0
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74928880"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Adición de un enlace de cola de Azure Storage a la función de Python

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

En este artículo se muestra cómo integrar la función que creó en el [artículo de inicio rápido anterior](functions-create-first-function-python.md) con una cola de Azure Storage. El enlace de salida que se agrega a esta función escribe datos de una solicitud HTTP en un mensaje de la cola.

La mayoría de los enlaces requieren una cadena de conexión almacenada que se usa en Functions para acceder al servicio enlazado. Para facilitar la conexión, usará la cuenta de almacenamiento que creó con la aplicación de funciones. La conexión a esta cuenta ya está almacenada en una configuración de aplicación llamada `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este artículo, realice los pasos de la [parte 1 del inicio rápido de Python](functions-create-first-function-python.md).

[!INCLUDE [functions-cloud-shell-note](../../includes/functions-cloud-shell-note.md)]

## <a name="download-the-function-app-settings"></a>Descarga de la configuración de la aplicación de función

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Habilitación de conjuntos de extensiones

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Ahora podrá agregar el enlace de salida de almacenamiento al proyecto.

## <a name="add-an-output-binding"></a>Adición de un enlace de salida

En Functions, para cada tipo de enlace es necesario definir los elementos `direction`, `type` y un valor único de `name` en el archivo function.json. La manera de definir estos atributos depende del lenguaje de la aplicación de funciones.

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Adición de código que utilice el enlace de salida

[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]

Al usar un enlace de salida, no tiene que usar el código del SDK de Azure Storage para autenticarse, obtener una referencia de cola o escribir datos. El sistema en tiempo de ejecución de Functions y el enlace de salida de cola realizan esas tareas automáticamente.

## <a name="run-the-function-locally"></a>Ejecución local de la función

Como antes, use el siguiente comando para iniciar el sistema en tiempo de ejecución de Functions localmente:

```bash
func host start
```

> [!NOTE]  
> Como ya habilitó las agrupaciones de extensiones en el archivo host.json, la [extensión de enlace de Storage](functions-bindings-storage-blob.md#packages---functions-2x-and-higher) se descargó e instaló automáticamente durante el inicio junto con el resto de extensiones de enlace de Microsoft.

Copie la dirección URL de la función `HttpTrigger` que aparece en la salida en entorno de ejecución y péguela en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. Verá la misma respuesta en el explorador que en el artículo anterior.

Esta vez, el enlace de salida también crea una cola denominada `outqueue` en la cuenta de almacenamiento y agrega un mensaje con esta misma cadena.

A continuación, se usa la CLI de Azure para ver la nueva cola y comprobar que se ha agregado un mensaje. También puede ver la cola mediante el [Explorador de Microsoft Azure Storage][Azure Storage Explorer] o en [Azure Portal](https://portal.azure.com).

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Reimplementación del proyecto 

Para actualizar la aplicación publicada, use el comando [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) de Core Tools para implementar el código del proyecto en Azure. En este ejemplo, reemplace `<APP_NAME>` por el nombre de la aplicación.

```command
func azure functionapp publish <APP_NAME> --build remote
```

De nuevo, puede usar cURL o un explorador para probar la función implementada. Como antes, anexe la cadena de consulta `&name=<yourname>` a la dirección URL, como en este ejemplo:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

Puede [examinar el mensaje de la cola de almacenamiento](#query-the-storage-queue) otra vez para comprobar que el enlace de salida genera un nuevo mensaje en la cola según lo previsto.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha actualizado la función desencadenada por HTTP para escribir datos en una cola de almacenamiento. Para más información sobre el desarrollo de Azure Functions con Python, consulte la [Guía de Azure Functions para desarrolladores de Python](functions-reference-python.md) y [Desencadenadores y enlaces en Azure Functions](functions-triggers-bindings.md). Para obtener ejemplos de proyectos de función completos en Python, consulte los ejemplos de [funciones de Python](/samples/browse/?products=azure-functions&languages=python). Para obtener más información sobre los precios, consulte la [página de precios de Functions](https://azure.microsoft.com/pricing/details/functions/) y el artículo [Estimación de los costos según el plan de consumo](functions-consumption-costs.md).

A continuación, debe habilitar la supervisión de Application Insights para su aplicación de función:

> [!div class="nextstepaction"]
> [Habilitación de la integración de Application Insights](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/