---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: e319356d555f26354ea29dc7be068bf6168abb17
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455151"
---
## <a name="local-settings-file"></a>Archivo de configuración local

El archivo local.settings.json almacena la configuración de la aplicación, las cadenas de conexión y la configuración que usan las herramientas de desarrollo local. Las herramientas solo usan las opciones de configuración de dicho archivo cuando estas se ejecutan localmente. El archivo de configuración local tiene la siguiente estructura:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

Los siguientes ajustes son compatibles cuando se ejecuta localmente:

| Configuración      | DESCRIPCIÓN                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Cuando se establece en `true`, todos los valores se cifran con una clave de máquina local. Se usa con los comandos `func settings`. El valor predeterminado es `false`. |
| **`Values`** | Matriz de opciones de configuración de la aplicación y cadenas de conexión que se usan en la ejecución local. Estos pares clave-valor (cadena-cadena) corresponden a la configuración de la aplicación en su aplicación de funciones de Azure, como [`AzureWebJobsStorage`]. Muchos desencadenadores y enlaces tienen una propiedad que hace referencia a una configuración de la aplicación de cadena de conexión como, por ejemplo, `Connection`para el [desencadenador de Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---configuration). Para estas propiedades, se necesita una configuración de la aplicación definida en la matriz `Values`. <br/>[`AzureWebJobsStorage`] es una configuración de aplicación necesaria para los desencadenadores que no sean HTTP. <br/>La versión 2.x del tiempo de ejecución de Functions requiere la configuración [`FUNCTIONS_WORKER_RUNTIME`] que Core Tools genera para su proyecto. <br/> Cuando tenga el [emulador de Azure Storage](../articles/storage/common/storage-use-emulator.md) instalado localmente, puede establecer [`AzureWebJobsStorage`] en `UseDevelopmentStorage=true` y Core Tools usará el emulador. Esto es útil durante el desarrollo, pero debe probar con una conexión de almacenamiento real antes de la implementación.<br/> Los valores deben ser cadenas y no objetos JSON o matrices. Los nombres de configuración no pueden incluir dos puntos (`:`) o un doble subrayado (`__`); estos están reservados para el tiempo de ejecución.  |
| **`Host`** | La configuración que se muestra esta sección permite personalizar el proceso de host de Functions cuando se ejecuta localmente. Estos son independientes de la configuración de host.json, que también se aplican cuando se ejecuta en Azure. |
| **`LocalHttpPort`** | Establece el puerto predeterminado que se usa cuando al ejecutar el host de Functions local (`func host start` y `func run`). La opción de línea de comandos `--port` tiene prioridad sobre este valor. |
| **`CORS`** | Define los orígenes permitidos para el [uso compartido de recursos entre orígenes (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Los orígenes se proporcionan en una lista de valores separados por comas y sin espacios. Se admite el valor comodín (\*), lo que permite realizar solicitudes desde cualquier origen. |
| **`CORSCredentials`** |  Establézcalo en "true" para permitir `withCredentials` solicitudes. |
| **`ConnectionStrings`** | No utilice esta colección para las cadenas de conexión que utilizan los enlaces de función. Esta colección solo la usan marcos que habitualmente obtienen las cadenas de conexión desde la sección `ConnectionStrings` de un archivo de configuración, como [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Las cadenas de conexión de este objeto se agregan al entorno con el tipo de proveedor de [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Los elementos de esta colección no se publican en Azure con otra configuración de aplicación. Debe agregar explícitamente estos valores a la colección `Connection strings` de la configuración de la aplicación de función. Si está creando un objeto [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) en el código de la función, debe almacenar el valor de la cadena de conexión en **Configuración de la aplicación** con las otras conexiones. |

["AzureWebJobsStorage"]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
