---
title: Referencia de configuración de aplicación para Azure Functions
description: Documentación de referencia para la configuración de la aplicación de Azure Functions o de variables de entorno.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/22/2018
ms.author: glenga
ms.openlocfilehash: ec309bc5484c0ac96d1323c56670c147737e7c64
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377870"
---
# <a name="app-settings-reference-for-azure-functions"></a>Referencia de configuración de aplicación para Azure Functions

La configuración de la aplicación en una aplicación de función contiene opciones de configuración global que afectan a todas las funciones de dicha aplicación. Cuando se ejecuta localmente, esta configuración es en [variables de entorno](functions-run-local.md#local-settings-file). Este artículo incluye una lista de las opciones de configuración disponibles en las aplicaciones de funciones.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Hay otras opciones de configuración global en el archivo [host.json](functions-host-json.md) y en [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="appinsightsinstrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Clave de instrumentación de Application Insights si usa dicho servicio. Consulte [Supervisar Azure Functions](functions-monitoring.md).

|Clave|Valor de ejemplo|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Cadena de conexión de la cuenta de almacenamiento opcional para almacenar los registros y mostrarlos en la pestaña **Monitor** del portal. La cuenta de almacenamiento debe ser una de uso general que admite blobs, colas y tablas. Consulte [Almacenamiento de la cuenta](functions-infrastructure-as-code.md#storage-account) y [Requisitos de almacenamiento de la cuenta](functions-create-function-app-portal.md#storage-account-requirements).

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol=https;AccountName=[nombre];AccountKey=[clave]|

> [!TIP]
> Por motivos de rendimiento y experiencia, se recomienda utilizar APPINSIGHTS_INSTRUMENTATIONKEY y App Insights para la supervisión, en lugar de AzureWebJobsDashboard.

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` significa deshabilitar la página de aterrizaje predeterminada que se muestra para la dirección URL raíz de una aplicación de función. El valor predeterminado es `false`.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Cuando esta configuración de la aplicación se omite o se establece en `false`, una página similar a la del siguiente ejemplo se muestra en respuesta a la dirección URL `<functionappname>.azurewebsites.net`.

![Página de aterrizaje de la aplicación de función](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` significa utilizar el modo de versión cuando se compila código .NET y `false` significa utilizar el modo de depuración. El valor predeterminado es `true`.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Una lista delimitada por comas de características de la versión beta que se van a habilitar. Las características de la versión beta habilitadas por estas marcas no están listas para la producción, pero se pueden habilitar para su uso experimental antes de su publicación.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsFeatureFlags|feature1,feature2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

Ruta de acceso al directorio raíz donde se encuentran las carpetas de función y el archivo *host.json*. En una aplicación de función, el valor predeterminado es `%HOME%\site\wwwroot`.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsScriptRoot|%HOME%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Especifica el repositorio o el proveedor que se utilizará para el almacenamiento de claves. Actualmente, los repositorios admitidos son blob ("Blob") y el sistema de archivos ("disabled"). El valor predeterminado es el sistema de archivos ("disabled").

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsSecretStorageType|deshabilitado|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

El tiempo de ejecución de Azure Functions usa esta cadena de conexión de la cuenta de almacenamiento para todas las funciones, salvo en el caso de las desencadenadas de HTTP. La cuenta de almacenamiento debe ser una de uso general que admite blobs, colas y tablas. Consulte [Almacenamiento de la cuenta](functions-infrastructure-as-code.md#storage-account) y [Requisitos de almacenamiento de la cuenta](functions-create-function-app-portal.md#storage-account-requirements).

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol=https;AccountName=[nombre];AccountKey=[clave]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

Ruta de acceso al compilador que se usa para TypeScript. Le permite reemplazar el valor predeterminado si lo necesita.

|Clave|Valor de ejemplo|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="functionappeditmode"></a>MODO DE\_EDICIÓN DE\_LA APLICACIÓN DE\_FUNCIÓN

Los valores válidos son "readwrite" y "readonly".

|Clave|Valor de ejemplo|
|---|------------|
|MODO DE\_EDICIÓN DE\_LA APLICACIÓN DE\_FUNCIÓN|readonly|

## <a name="functionsextensionversion"></a>VERSIÓN DE LA \_EXTENSIÓN\_ DE FUNCTIONS

La versión del tiempo de ejecución de Functions para usar en esta aplicación de función. Una tilde con la versión principal significa utilizar la versión más reciente de esa versión principal (por ejemplo, "~2"). Cuando haya disponibles versiones nuevas de la misma versión principal, se instalarán automáticamente en la aplicación de función. Para anclar la aplicación a una versión específica, use el número completo de la versión (por ejemplo, "2.0.12345"). El valor predeterminado es "~2". Un valor de `~1` ancla la aplicación a la versión 1.x del tiempo de ejecución.

|Clave|Valor de ejemplo|
|---|------------|
|VERSIÓN DE LA \_EXTENSIÓN\_ DE FUNCTIONS|~2|

## <a name="functionsworkerruntime"></a>FUNCTIONS\_WORKER\_RUNTIME

Tiempo de ejecución del trabajo del lenguaje que se cargará en la aplicación de función.  Se corresponderá con el lenguaje usado en la aplicación (por ejemplo, "dotnet"). Para las funciones en varios lenguajes deberá publicarlas en varias aplicaciones, cada una con un valor de tiempo de ejecución de trabajo correspondiente.  Los valores válidos son `dotnet` (C#/F#), `node` (JavaScript) y `java` (Java).

|Clave|Valor de ejemplo|
|---|------------|
|FUNCTIONS\_WORKER\_RUNTIME|dotnet|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Solo para los planes de consumo. Cadena de conexión para la cuenta de almacenamiento donde se almacenan el código de aplicación de función y la configuración. Consulte [Creación de una aplicación de función](functions-infrastructure-as-code.md#create-a-function-app).

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol=https;AccountName=[nombre];AccountKey=[clave]|

## <a name="websitecontentshare"></a>WEBSITE\_CONTENTSHARE

Solo para los planes de consumo. Ruta de acceso del archivo para el código de la aplicación de función y la configuración. Se usa con WEBSITE_CONTENTAZUREFILECONNECTIONSTRING. El valor predeterminado es una cadena única que comienza con el nombre de aplicación de función. Consulte [Creación de una aplicación de función](functions-infrastructure-as-code.md#create-a-function-app).

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>ESCALABILIDAD\_HORIZONTAL\_MÁXIMA\_DE LA\_APLICACIÓN\_DINÁMICA

Número máximo de instancias al que se puede escalar horizontalmente la aplicación de función. El valor predeterminado es sin límite.

> [!NOTE]
> Este ajuste es una característica en vista previa, que solo es confiable si se establece en un valor <= 5

|Clave|Valor de ejemplo|
|---|------------|
|ESCALABILIDAD\_HORIZONTAL\_MÁXIMA\_DE LA\_APLICACIÓN\_DINÁMICA|5|

## <a name="websitenodedefaultversion"></a>DEFAULT_VERSION\_DEL NODO\_DEL SITIO WEB

El valor predeterminado es "8.11.1".

|Clave|Valor de ejemplo|
|---|------------|
|DEFAULT_VERSION\_DEL NODO\_DEL SITIO WEB|8.11.1|

## <a name="websiterunfrompackage"></a>WEBSITE\_RUN\_FROM\_PACKAGE

Permite que la aplicación de función se ejecute desde un archivo de paquete montado.

|Clave|Valor de ejemplo|
|---|------------|
|WEBSITE\_RUN\_FROM\_PACKAGE|1|

Los valores válidos son una dirección URL que se resuelve en la ubicación de un archivo de paquete de implementación o `1`. Cuando se establece en `1`, el paquete debe estar en la carpeta `d:\home\data\SitePackages`. Cuando se usa la implementación de ZIP con esta configuración, el paquete se carga automáticamente en esta ubicación. En la versión preliminar, este ajuste se denomina `WEBSITE_RUN_FROM_ZIP`. Para más información, vea [Run your functions from a package file](run-functions-from-deployment-package.md) (Ejecución de Azure Functions desde un archivo de paquete).

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información acerca de cómo actualizar la configuración de la aplicación](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

[Consulte la configuración global en el archivo host.json](functions-host-json.md)

[Consulte otros valores de aplicación para aplicaciones de App Service](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
