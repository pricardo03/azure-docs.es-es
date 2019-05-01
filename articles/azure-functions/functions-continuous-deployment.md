---
title: Implementación continua para Azure Functions | Microsoft Docs
description: Utilice las funciones de implementación continua de Azure App Service para publicar sus funciones.
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
ms.openlocfilehash: cb3f3ad3bb7b42429654ea4bf9b49f7e230db1da
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943882"
---
# <a name="continuous-deployment-for-azure-functions"></a>Implementación continua para Azure Functions
Las funciones de Azure permite fácilmente implementar la aplicación de función mediante la integración continua. Functions se integra con los repositorios de código principal y orígenes de implementación. Esta integración permite a un flujo de trabajo que actualiza el código de función realizada a través de uno de estos servicios desencadenan la implementación en Azure. Si está familiarizado con Azure Functions, comience con la [información general sobre Azure Functions](functions-overview.md).

La implementación continua es una buena opción para los proyectos donde está realizando la integración múltiples y frecuentes de contribuciones. También le permite mantener el control de código fuente en el código de función. Azure Functions admite los siguientes orígenes de implementación:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Repositorio externo (Git o Mercurial)
* [Repositorio local de GIT](../app-service/deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Azure DevOps](https://azure.microsoft.com/services/devops/)

Las implementaciones se configuran por Function App. Después de habilitada la implementación continua, el acceso al código de la función en el portal está establecido en acceso *de solo lectura*.

## <a name="requirements-for-continuous-deployment"></a>Requisitos para la implementación continua

Antes de configurar la implementación continua, debe tener configurado el origen de la implementación y el código de función en el origen de implementación. En una implementación de aplicación de función, cada función está en un subdirectorio con nombre, donde el nombre del directorio es el nombre de la función.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Para poder realizar una implementación desde Azure DevOps, primero debe vincular la organización de Azure DevOps a la suscripción de Azure. Para más información, consulte [Configuración de la facturación para una organización de Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/billing/set-up-billing-for-your-organization-vs?view=vsts#set-up-billing-via-the-azure-portal).

## <a name="set-up-continuous-deployment"></a>Configurar la implementación continua
Use este procedimiento para configurar la implementación continua para una Function App existente. Estos pasos muestran la integración con un repositorio de GitHub, aunque se aplican pasos similares para Azure DevOps u otros servicios de implementación.

1. En la aplicación de función en el [portal Azure](https://portal.azure.com), seleccione **características de la plataforma** > **opciones de implementación**. 
   
    ![Selecciones para abrir Opciones de implementación](./media/functions-continuous-deployment/setup-deployment.png)
 
1. En el **implementaciones** hoja, seleccione **instalación**.
 
    ![Hoja de implementaciones](./media/functions-continuous-deployment/setup-deployment-1.png)
   
1. En el **origen de implementación** hoja, seleccione **Elegir origen**. Rellene la información para el origen de implementación elegido y, a continuación, seleccione **Aceptar**.
   
    ![Elegir un origen de implementación](./media/functions-continuous-deployment/choose-deployment-source.png)

Después de configurar la implementación continua, se copian todos los cambios de archivo en el origen de implementación en la aplicación de función y se desencadena una implementación completa del sitio. Cuando se actualizan los archivos del origen se vuelve a implementar el sitio.

## <a name="deployment-scenarios"></a>Escenarios de implementación

Escenarios típicos de implementación incluyen la creación de una implementación de ensayo y mover las funciones existentes para la implementación continua.

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Creación de una implementación de ensayo

Aún no admiten las ranuras de implementación de aplicaciones de función. Pero todavía puede administrar las implementaciones de ensayo y producción independientes mediante la integración continua.

El proceso para configurar y trabajar con una implementación de ensayo tiene normalmente este aspecto:

1. Crear dos aplicaciones de función en su suscripción: uno para el código de producción y otro para el almacenamiento provisional. 

1. Cree un origen de la implementación, si aún no tiene uno. En este ejemplo se usa [GitHub].

1. Para la Function App de producción, complete los pasos anteriores descritos en [Configuración de la implementación continua](#set-up-continuous-deployment) y establezca la rama de implementación en la rama principal del repositorio de GitHub.
   
    ![Selecciones para elegir una rama de implementación](./media/functions-continuous-deployment/choose-deployment-branch.png)

1. Repita el paso 3 para la aplicación de función de ensayo, pero seleccione la rama de ensayo en su lugar en el repositorio de GitHub. Si el origen de la implementación no admite la bifurcación, utilice una carpeta diferente.
    
1. Realizar actualizaciones en el código en la rama o carpeta de almacenamiento provisional y, a continuación, compruebe que la implementación de ensayo refleja estos cambios.

1. Después de la prueba, combine los cambios de la rama de ensayo en la rama principal. Esta combinación desencadena la implementación en la Function App de producción. Si el origen de la implementación no admite ramas, sobrescriba los archivos de la carpeta de producción con los archivos de la carpeta de ensayo.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Movimiento de funciones existentes para la implementación continua
Cuando se dispone de funciones existentes que ha creado y mantenido en el portal, necesita descargar los archivos de código de función mediante FTP o el repositorio de Git local antes de puede configurar la implementación continua como se describió anteriormente. Puede hacerlo en la configuración de Azure App Service para la aplicación de función. Después de descargar los archivos, puede cargarlos en el origen de implementación continua que haya seleccionado.

> [!NOTE]
> Después de configurar la integración continua, ya no se pueden editar los archivos de origen en el portal de Functions.

<a name="credentials"></a>
#### <a name="configure-deployment-credentials"></a>Configuración de las credenciales de implementación
Antes de poder descargar archivos desde la aplicación de función mediante FTP o un repositorio Git local, debe configurar las credenciales para acceder al sitio. Las credenciales se establecen en el nivel de aplicación de función. Use los pasos siguientes para establecer las credenciales de implementación en Azure Portal:

1. En la aplicación de función en el [portal Azure](https://portal.azure.com), seleccione **características de la plataforma** > **credenciales de implementación**.
   
1. Escriba un nombre de usuario y una contraseña y, a continuación, seleccione **guardar**. 

   ![Selecciones para establecer las credenciales de implementación local](./media/functions-continuous-deployment/setup-deployment-credentials.png)

Ahora puede usar estas credenciales para tener acceso a la aplicación de función mediante FTP o desde el repositorio de Git integrado.

<a name="downftp"></a>
#### <a name="download-files-by-using-ftp"></a>Descargar archivos mediante FTP

1. En la aplicación de función en el [portal Azure](https://portal.azure.com), seleccione **características de la plataforma** > **propiedades**. A continuación, copie los valores de **FTP/usuario de implementación**, **nombre de Host FTP**, y **nombre del Host FTPS**.  

   **FTP/usuario de implementación** debe escribirse tal como aparece en el portal, incluido el nombre de la aplicación, a fin de proporcionar el contexto adecuado para el servidor FTP.
   
   ![Selecciones para obtener la información de implementación](./media/functions-continuous-deployment/get-deployment-credentials.png)

1. Desde el cliente FTP, use la información de conexión recopilada para conectarse a la aplicación y descargar los archivos de origen para las funciones.

<a name="downgit"></a>
#### <a name="download-files-by-using-a-local-git-repository"></a>Descargar archivos mediante un repositorio Git local

1. En la aplicación de función en el [portal Azure](https://portal.azure.com), seleccione **características de la plataforma** > **opciones de implementación**. 
   
    ![Selecciones para abrir Opciones de implementación](./media/functions-continuous-deployment/setup-deployment.png)
 
1. A continuación, en el **implementaciones** hoja, seleccione **instalación**.
 
    ![Hoja de implementaciones](./media/functions-continuous-deployment/setup-deployment-1.png)
   
1. En el **origen de implementación** hoja, seleccione **repositorio de Git Local** > **Aceptar**.

1. En **características de la plataforma**, seleccione **propiedades** y anote el valor de la dirección URL de Git. 
   
    ![Selecciones para obtener la dirección URL de Git](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

1. Clone el repositorio en el equipo local mediante un símbolo del sistema compatible con Git o su herramienta Git favorita. El comando clone de Git tiene este aspecto:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

1. Recupere los archivos de su aplicación de función y póngalos en la copia del equipo local, como en el ejemplo siguiente:
   
        git pull origin master
   
    Si se le piden, proporcione las [credenciales de implementación configuradas](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
