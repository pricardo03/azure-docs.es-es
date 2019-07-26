---
title: Tecnologías de implementación en Azure Functions | Microsoft Docs
description: Conozca las distintas formas de implementación de código en Azure Functions.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 47d8bf33fd686942326db3b1cc606978bf47a1bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594392"
---
# <a name="deployment-technologies-in-azure-functions"></a>Tecnologías de implementación en Azure Functions

Puede usar algunas tecnologías diferentes para implementar el código del proyecto de Azure Functions en Azure. En este artículo se ofrece una lista exhaustiva de esas tecnologías, se informa de qué tecnologías están disponibles para los tipos de instancias de Functions, se explica lo que sucede cuando se usa cada método y se proporcionan recomendaciones del mejor método que usar en diversos escenarios. Las diversas herramientas que admiten la implementación en Azure Functions se ajustan a la tecnología correcta en función de su contexto.

## <a name="deployment-technology-availability"></a>Disponibilidad de la tecnología de implementación

> [!IMPORTANT]
> Azure Functions admite el desarrollo local multiplataforma y hospedaje en Windows y Linux. Actualmente, hay tres planes de hospedaje disponibles: [Consumo](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan) y [Dedicado (Azure App Service)](functions-scale.md#app-service-plan). Cada plan tiene diferentes comportamientos. No todas las tecnologías de implementación están disponibles para todos los tipos de instancia de Azure Functions.

| Tecnología de implementación | Consumo de Windows | Premium de Windows (versión preliminar) | Dedicado de Windows  | Consumo para Linux (versión preliminar) | Dedicado de Linux |
|-----------------------|:-------------------:|:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| Dirección URL del paquete externo<sup>1</sup> |✔|✔|✔|✔|✔|
| Implementación de archivo ZIP |✔|✔|✔| |✔|
| Contenedor de Docker | | | | |✔|
| Web Deploy |✔|✔|✔| | |
| Control de código fuente |✔|✔|✔| |✔|
| Git local<sup>1</sup> |✔|✔|✔| |✔|
| Sincronización en la nube<sup>1</sup> |✔|✔|✔| |✔|
| FTP<sup>1</sup> |✔|✔|✔| |✔|
| Edición del portal |✔|✔|✔| |✔<sup>2</sup>|

<sup>1</sup> Tecnología de implementación que requiere [sincronización manual de desencadenadores](#trigger-syncing).  
<sup>2</sup> La edición del portal solo está habilitada para los desencadenadores de HTTP y de temporizador para Functions en Linux con el plan Dedicado.

## <a name="key-concepts"></a>Conceptos clave

Algunos conceptos clave son fundamentales para comprender cómo funcionan las implementaciones en Azure Functions.

### <a name="trigger-syncing"></a>Sincronización de desencadenadores

Cuando se modifica cualquiera de los desencadenantes, la infraestructura de Funciones debe estar al tanto de los cambios. La sincronización se realiza automáticamente para muchas tecnologías de implementación. Aunque en algunos casos habrá que sincronizar manualmente los desencadenadores. Si al implementar las actualizaciones se hace referencia a una dirección URL del paquete externo, Git local, sincronización en la nube o FTP, hay que sincronizar manualmente los desencadenadores. Puede sincronizar los desencadenadores de una de estas tres maneras:

* Reinicie la aplicación de funciones en Azure Portal.
* Envíe una solicitud HTTP POST a `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` mediante la [clave maestra](functions-bindings-http-webhook.md#authorization-keys).
* Envíe la solicitud HTTP POST a `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Reemplace los marcadores de posición con el identificador de suscripción, el nombre del grupo de recursos y el nombre de la aplicación de funciones.

## <a name="deployment-technology-details"></a>Detalles de la tecnología de implementación 

En Azure Functions se encuentran disponibles los métodos de implementación siguientes.

### <a name="external-package-url"></a>Dirección URL del paquete externo

Puede utilizar la dirección URL del paquete externo para hacer referencia a un archivo (.zip) de paquete remoto que contiene la aplicación de funciones. El archivo se descarga desde la dirección URL proporcionada y la aplicación se ejecuta en modo de [ejecución desde el paquete](run-functions-from-deployment-package.md).

>__Cómo se debe usar:__ Agregue `WEBSITE_RUN_FROM_PACKAGE` a la configuración de la aplicación. El valor de esta opción debe ser una dirección URL (la ubicación del archivo de paquete específico que quiere ejecutar). Puede agregar una configuración cualquiera [en el portal](functions-how-to-use-azure-function-app-settings.md#settings) o [mediante la CLI de Azure](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). 
>
>Si usa Azure Blob Storage, utilice un contenedor privado con una [firma de acceso compartido (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) para que Functions tenga acceso al paquete. Cada vez que se reinicia la aplicación, se captura una copia del contenido. La referencia debe ser válida durante la vigencia de la aplicación.

>__Cuándo se debe usar__: La dirección URL del paquete externo es el único método de implementación compatible con Azure Functions que se ejecuta en Linux en el Plan de consumo (versión preliminar). Al actualizar el archivo de paquete al que hace referencia una aplicación de funciones, debe [sincronizar manualmente los desencadenadores](#trigger-syncing) para indicar a Azure que la aplicación ha cambiado.

### <a name="zip-deploy"></a>Implementación de archivo ZIP

Utilice la implementación de archivo ZIP para insertar un archivo ZIP que contiene la aplicación de funciones de Azure. También puede establecer que la aplicación se inicie en modo de [ejecución desde el paquete](run-functions-from-deployment-package.md).

>__Cómo se debe usar:__ Realice la implementación con su herramienta cliente favorita: [VS Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure) la [CLI de Azure](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Para implementar manualmente un archivo ZIP en la aplicación de funciones, siga las instrucciones que encontrará en [Deploying from a zip file or url](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url) (Implementación desde un archivo ZIP o una dirección URL).
>
>Si realiza la implementación mediante la implementación zip, puede establecer la aplicación para que se ejecute en modo de [ejecución desde el paquete](run-functions-from-deployment-package.md). Para establecer el modo de ejecución desde el paquete, defina el valor de configuración de la aplicación `WEBSITE_RUN_FROM_PACKAGE` en `1`. Se recomienda usar la implementación de archivos ZIP. Produce tiempos de carga más rápidos para las aplicaciones, y es el valor predeterminado para VS Code, Visual Studio y la CLI de Azure.

>__Cuándo se debe usar__: Zip Deploy es la tecnología de implementación recomendada para la ejecución de Azure Functions en Windows y en Linux en el plan Dedicado.

### <a name="docker-container"></a>Contenedor de Docker

Puede implementar una imagen de contenedor de Linux que contenga la aplicación de funciones.

>__Cómo se debe usar:__ Cree una aplicación de funciones de Linux en el plan Dedicado y especifique desde qué imagen de contenedor se debe ejecutar. Puede hacerlo de dos maneras:
>
>* Cree una aplicación de funciones de Linux en un plan de Azure App Service en Azure Portal. En **Publicar**, seleccione **Imagen de Docker** y configure el contenedor. Escriba la ubicación donde se hospeda la imagen.
>* Cree una aplicación de funciones de Linux en un plan de App Service mediante la CLI de Azure. Para obtener más información, consulte [Creación de una función en Linux con una imagen personalizada](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Para implementar una aplicación existente mediante un contenedor personalizado, use el comando [`func deploy`](functions-run-local.md#publish) de [Azure Functions Core Tools](functions-run-local.md).

>__Cuándo se debe usar__: Use la opción de contenedor de Docker cuando quiera tener más control sobre el entorno de Linux donde se ejecuta la aplicación de funciones. Este mecanismo de implementación solo está disponible para las instancias de Functions que se ejecutan en Linux en un plan de App Service.

### <a name="web-deploy-msdeploy"></a>Web Deploy (MSDeploy)

Web Deploy empaqueta e implementa las aplicaciones Windows en cualquier servidor IIS, incluidas las aplicaciones de funciones que se ejecutan en Windows en Azure.

>__Cómo se debe usar:__ Use [Visual Studio Tools para Azure Functions](functions-create-your-first-function-visual-studio.md). Desactive la casilla **Ejecutar desde el archivo de paquete (recomendado)** .
>
>También puede descargar [Web Deploy 3.6](https://www.iis.net/downloads/microsoft/web-deploy) y llamar a `MSDeploy.exe` directamente.

>__Cuándo se debe usar__: Web Deploy se admite y no presenta problemas, pero el mecanismo preferido ahora es la [implementación de archivo ZIP con la ejecución desde el paquete habilitada](#zip-deploy). Para obtener más información, consulte la [guía de desarrollo de Visual Studio](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Control de código fuente

Use el control de código fuente para conectar la aplicación de funciones a un repositorio de Git. Una actualización del código de ese repositorio desencadena la implementación. Para más información, vea la [wiki de Kudu](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Cómo se debe usar:__ Use el centro de implementación del portal de Azure Functions para configurar la publicación desde el control de código fuente. Para más información, vea [Implementación continua para Azure Functions](functions-continuous-deployment.md).

>__Cuándo se debe usar__: El uso del control de código fuente es el procedimiento recomendado para los equipos que colaboran en sus aplicaciones de funciones. El control de código fuente es una buena opción de implementación que permite el uso de canalizaciones de implementación más sofisticadas.

### <a name="local-git"></a>Git local

Puede usar GIT local para insertar código desde la máquina local en Azure Functions.

>__Cómo se debe usar:__ Siga las instrucciones de [Implementación de Git local a Azure App Service](../app-service/deploy-local-git.md).

>__Cuándo se debe usar__: En general, se recomienda usar otro método de implementación. Al publicar desde Git local, debe [sincronizar manualmente los desencadenadores](#trigger-syncing).

### <a name="cloud-sync"></a>Sincronización en la nube

Use la sincronización en la nube para sincronizar el contenido desde Dropbox y OneDrive para Azure Functions.

>__Cómo se debe usar:__ Siga las instrucciones de [Sincronización de contenido de una carpeta de nube](../app-service/deploy-content-sync.md).

>__Cuándo se debe usar__: En general, se recomiendan otros métodos de implementación. Al publicar mediante la sincronización en la nube, debe [sincronizar manualmente los desencadenadores](#trigger-syncing).

### <a name="ftp"></a>FTP

Puede usar FTP para transferir archivos directamente a Azure Functions.

>__Cómo se debe usar:__ Siga las instrucciones de [Implementación de contenido mediante FTP/S](../app-service/deploy-ftp.md).

>__Cuándo se debe usar__: En general, se recomiendan otros métodos de implementación. Al publicar mediante FTP, debe [sincronizar manualmente los desencadenadores](#trigger-syncing).

### <a name="portal-editing"></a>Edición del portal

En el editor basado en el portal, puede editar directamente los archivos que se encuentran en la aplicación de funciones (en esencia, se implementan cada vez que guarde los cambios).

>__Cómo se debe usar:__ Para poder editar las funciones en Azure Portal, debe haber [creado las funciones en el portal](functions-create-first-azure-function.md). Para conservar un único origen de confianza, el uso de cualquier otro método de implementación hace que la función sea de solo lectura e impide la edición del portal de forma continuada. Para volver a un estado en el que pueda editar los archivos en Azure Portal, puede volver a activar manualmente el modo de edición en `Read/Write` y quitar la configuración de la aplicación relacionada con la implementación (como `WEBSITE_RUN_FROM_PACKAGE`). 

>__Cuándo se debe usar__: El portal es una buena forma de empezar a trabajar con Azure Functions. Para un trabajo de desarrollo más intensivo, se recomienda usar las herramientas de cliente:
>
>* [Introducción al uso de VS Code](functions-create-first-function-vs-code.md)
>* [Introducción al uso de Azure Functions Core Tools](functions-run-local.md)
>* [Introducción al uso de Visual Studio](functions-create-your-first-function-visual-studio.md)

La siguiente tabla muestra los sistemas operativos y lenguajes que admiten la edición del portal:

| | Consumo de Windows | Premium de Windows (versión preliminar) | Dedicado de Windows | Consumo de Linux (versión preliminar) | Dedicado de Linux |
|-|:-----------------: |:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| C# | | | | | |
| Script de C# |✔|✔|✔| |✔<sup>*</sup>|
| F# | | | | | |
| Java | | | | | |
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>*</sup>|
| Python (versión preliminar) | | | | | |
| PowerShell (versión preliminar) |✔|✔|✔| | |
| TypeScript (Node.js) | | | | | |

<sup>*</sup> La edición del portal solo está habilitada para los desencadenadores de HTTP y de temporizador de Functions en Linux con el plan Dedicado.

## <a name="deployment-slots"></a>Ranuras de implementación

Al implementar la aplicación de funciones en Azure, puede implementar en una ranura de implementación independiente en lugar de directamente en producción. Para más información sobre las ranuras de implementación, consulte la [documentación de ranuras de Azure App Service](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Niveles de compatibilidad de las ranuras de implementación

Hay dos niveles de compatibilidad para las ranuras de implementación:

* **Disponibilidad general (GA)** : significa que es totalmente compatible y está aprobada para su uso en producción.
* **Versión preliminar**: aún no cuenta con soporte pero se espera que llegue al estado de disponibilidad general en el futuro.

| Sistema operativo o plan de hospedaje | Nivel de compatibilidad |
| --------------- | ------ |
| Consumo de Windows | Vista previa |
| Premium de Windows (versión preliminar) | Vista previa |
| Dedicado de Windows | Disponibilidad general |
| Consumo de Linux | No compatible |
| Dedicado de Linux | Disponibilidad general |

## <a name="next-steps"></a>Pasos siguientes

Lea los siguientes artículos para obtener más información sobre cómo implementar las aplicaciones de funciones: 

+ [Implementación continua para Azure Functions](functions-continuous-deployment.md)
+ [Entrega continua con Azure DevOps](functions-how-to-azure-devops.md)
+ [Implementación de archivos ZIP en Azure Functions](deployment-zip-push.md)
+ [Ejecución de la instancia de Azure Functions desde un archivo de paquete](run-functions-from-deployment-package.md)
+ [Automatización de la implementación de recursos para una aplicación de funciones en Azure Functions](functions-infrastructure-as-code.md)
