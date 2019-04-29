---
title: Registro y diagnóstico de Azure espacial delimitadores | Microsoft Docs
description: Explicación detallada de cómo generar y recuperar el registro y diagnóstico de Azure espacial delimitadores.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 134023c0884ce3a402b99806f1bf19dcb59ecc32
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564874"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Registro y diagnóstico de Azure espacial delimitadores

Delimitadores espacial Azure proporciona un mecanismo de registro estándar que es útil para el desarrollo de aplicaciones. El modo de registro de diagnósticos de delimitadores espacial es útil cuando necesita más información para la depuración. Registro de diagnóstico almacena las imágenes del entorno.

## <a name="standard-logging"></a>Registro estándar
En la API de delimitadores espacial, puede suscribirse al mecanismo de registro para obtener registros útiles para el desarrollo de aplicaciones y la depuración. Las API de registro estándar no almacene imágenes del entorno en el disco del dispositivo. El SDK proporciona estos registros como las devoluciones de llamada de evento. Depende de usted para integrar estos registros en el mecanismo de registro de la aplicación.

### <a name="configuration-of-log-messages"></a>Configuración de los mensajes de registro
Hay dos devoluciones de llamada de interés para el usuario. El ejemplo siguiente muestra cómo configurar la sesión.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events-and-properties"></a>Propiedades y eventos

Se proporcionan estas devoluciones de llamada de evento para procesar los registros y errores de la sesión:

- [LogLevel](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): Especifica el nivel de detalle de los eventos recibir desde el tiempo de ejecución.
- [OnLogDebug](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): Proporciona eventos de registro de depuración estándar.
- [Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): Proporciona eventos de registro que tiene en cuenta el tiempo de ejecución que haya errores.

## <a name="diagnostics-logging"></a>Registro de diagnóstico

Además el modo estándar de operación para el registro, los anclajes espaciales también tiene un modo de diagnóstico. Modo de diagnóstico captura imágenes del entorno y los registra en el disco. Puede usar este modo para depurar determinados tipos de problemas, como error al localizar un delimitador de forma predecible. Habilitar el registro de diagnóstico solo para reproducir un problema específico. A continuación, deshabilítelo. No habilite diagnósticos al ejecutar las aplicaciones con normalidad.

Durante una interacción de soporte técnico con Microsoft, un representante de Microsoft podría preguntarse si está dispuesto a enviar un lote de diagnóstico para una investigación más detallada. En este caso, podría decidir habilitar los diagnósticos y reproducir el problema para que pueda enviar el paquete de diagnóstico. 

Si envía un registro de diagnóstico a Microsoft sin confirmación anterior de un representante de Microsoft, el envío pasará sin responder.

Las secciones siguientes muestran cómo habilitar el modo de diagnóstico y también cómo enviar los registros de diagnóstico a Microsoft.

### <a name="enable-diagnostics-logging"></a>Habilitación del registro de diagnósticos

Cuando se habilita una sesión de registro de diagnóstico, todas las operaciones en la sesión tienen correspondiente registro de diagnóstico en el sistema de archivos local. Durante el registro, las imágenes del entorno se guardan en el disco.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // opt in to diagnostics logging of environment images
    // if this is enabled, the diagnostics bundle includes images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submit-the-diagnostics-bundle"></a>Enviar el paquete de diagnóstico

El fragmento de código siguiente muestra cómo enviar un paquete de diagnóstico a Microsoft. Este paquete incluye imágenes del entorno capturados por la sesión después de habilitar los diagnósticos. 

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="parts-of-a-diagnostics-bundle"></a>Partes de un paquete de diagnóstico
El paquete de diagnóstico podría contener la siguiente información:

- **Imágenes de fotograma clave**: Imágenes del entorno capturado durante la sesión mientras se han habilitado los diagnósticos.
- **Registros**: Registrar los eventos registrados por el tiempo de ejecución.
- **Los metadatos de la sesión**: Metadatos que identifica la sesión.
