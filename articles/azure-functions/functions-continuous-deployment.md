---
title: Implementación continua para Azure Functions | Microsoft Docs
description: Utilice las funciones de implementación continua de Azure App Service para publicar las funciones.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 2dc5fab0966c2ead0276cd8b23ea764bd4f9ef59
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190477"
---
# <a name="continuous-deployment-for-azure-functions"></a>Implementación continua para Azure Functions

Azure Functions permite implementar el código de forma continua a través la [integración del control de código fuente](functions-deployment-technologies.md#source-control). Esto hace posible un flujo de trabajo donde las actualizaciones de código desencadenan una implementación en Azure. Si no está familiarizado con Azure Functions, comience con [Información general sobre Azure Functions](functions-overview.md).

La implementación continua representa una buena opción para los proyectos donde se integran contribuciones diversas y frecuentes. También permite mantener un único origen de confianza del código de la función. La implementación continua en Azure Functions se puede configurar desde las siguientes ubicaciones de código fuente:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

La unidad de implementación de Azure Functions es la aplicación de funciones. Esto significa que todas las funciones de una aplicación de funciones se implementan al mismo tiempo. Tras habilitar la implementación continua, el acceso al código de función en Azure Portal se configura como de *solo lectura*, ya que el origen de confianza está establecido en otro lugar.

## <a name="requirements-for-continuous-deployment"></a>Requisitos de implementación continua

Para que la implementación continua se realice correctamente, la estructura de directorios debe ser compatible con la siguiente estructura básica de carpetas que Azure Functions espera:

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Configuración de la implementación continua

Use este procedimiento para configurar la implementación continua para una Function App existente. Estos pasos muestran la integración con un repositorio de GitHub, aunque estos mismos pasos son válidos en Azure Repos o en otros repositorios de código fuente.

1. En la aplicación de funciones, en [Azure Portal](https://portal.azure.com), seleccione **Características de la plataforma** > **Centro de implementación**.

    ![Abrir el centro de implementación](./media/functions-continuous-deployment/platform-features.png)

2. En el **Centro de implementación**, seleccione **GitHub** y, después, seleccione **Autorizar** o, si ya autorizó GitHub, seleccione **Continuar**. 

    ![Centro de implementación](./media/functions-continuous-deployment/github.png)

3. En GitHub, seleccione **Autorizar AzureAppService**. 

    ![Autorización](./media/functions-continuous-deployment/authorize.png)
    
    En el **Centro de implementación** de Azure Portal, seleccione **Continuar**.

4. Seleccione uno de los siguientes proveedores de compilación:

    * **Servicio de compilación de App Service**: opción ideal cuando no se necesita una compilación o se necesita una compilación genérica.
    * **Azure Pipelines (versión preliminar)** : opción ideal si se necesita más control sobre la compilación. Este proveedor está actualmente en versión preliminar.

    ![Selección de un proveedor de compilación](./media/functions-continuous-deployment/build.png)

5. Configure información específica de la opción de control de origen que haya indicado. En GitHub, debe proporcionar datos de la **Organización**, **Repositorio** y **Rama** donde el código se encuentra. Después, seleccione **Continuar**.

    ![Configuración de GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Por último, revise todos los detalles y seleccione **Finalizar** para completar la configuración de la implementación.

    ![Resumen](./media/functions-continuous-deployment/summary.png)

Cuando finalice el proceso, todo el código del origen especificado se implementará en la aplicación. Hecho esto, los cambios en el origen de implementación desencadenarán una implementación de dichos cambios en la aplicación de funciones en Azure.

## <a name="deployment-scenarios"></a>Escenarios de implementación

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Movimiento de funciones existentes para la implementación continua

Si ya ha escrito funciones en [Azure Portal](https://portal.azure.com) y quiere descargar el contenido de la aplicación antes de cambiar a la implementación continua, debe ir a la pestaña **Introducción** de la aplicación de funciones y hacer clic en el botón **Descargar contenido de aplicación**.

![Descarga de contenido de la aplicación](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Después de configurar la integración continua, ya no podrá modificar los archivos de origen en el portal de Functions.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
