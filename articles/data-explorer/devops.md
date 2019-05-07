---
title: Tarea de Azure DevOps para el Explorador de datos de Azure
description: En este tema, aprenderá a crear una canalización de versiones e implementar
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 05/05/2019
ms.openlocfilehash: a70a887ccb19d9c1cbdb5f8ebf6aa8d4b25a0dfd
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65161076"
---
# <a name="azure-devops-task-for-azure-data-explorer"></a>Tarea de Azure DevOps para el Explorador de datos de Azure

[Servicios de Azure DevOps](https://azure.microsoft.com/services/devops/) proporciona herramientas de colaboración, como las canalizaciones de alto rendimiento, repositorios Git privados gratuitos, paneles Kanban configurables y amplias capacidades de pruebas automatizadas y continuas de desarrollo. [Las canalizaciones de Azure](https://azure.microsoft.com/services/devops/pipelines/) es una funcionalidad de DevOps de Azure que le permite administrar CI/CD para implementar el código con las canalizaciones de alto rendimiento que funcionan con cualquier lenguaje y plataforma en la nube.
[Explorador de datos de Azure: comandos de administrador](https://marketplace.visualstudio.com/items?itemName=Azure-Kusto.PublishToADX) es la tarea de canalizaciones de Azure que permite crear canalizaciones de versiones e implementación de la base de datos cambia a las bases de datos del explorador de datos de Azure. Está disponible gratuitamente en el [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

Este documento describe un ejemplo sencillo en el uso de la **Explorador de datos de Azure: comandos de administrador** cambios de tareas para implementar el esquema en la base de datos. Para las canalizaciones de CI/CD completadas, consulte [documentación de Azure DevOps](/azure/devops/user-guide/what-is-azure-devops?view=azure-devops#vsts).

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* Configuración de clúster del explorador de datos de Azure:
    * Un [clúster del explorador de datos de Azure y base de datos](/azure/data-explorer/create-cluster-database-portal)
    * Crear aplicación de Azure Active Directory (Azure AD) mediante [aprovisionamiento de una aplicación de Azure AD](/azure/kusto/management/access-control/how-to-provision-aad-app).
    * Conceder acceso a la aplicación de Azure AD en la base de datos del explorador de datos de Azure por [administrar permisos de base de datos del explorador de Azure Data](/azure/data-explorer/manage-database-permissions).
* Configuración de DevOps de Azure:
    * [Regístrese para una organización gratis](/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops)
    * [Creación de una organización](/azure/devops/organizations/accounts/create-organization?view=azure-devops)
    * [Crear un proyecto de DevOps de Azure](/azure/devops/organizations/projects/create-project?view=azure-devops)
    * [Código con Git](/azure/devops/user-guide/code-with-git?view=azure-devops)

## <a name="create-folders"></a>Crear carpetas

Cree las siguientes carpetas de ejemplo (*funciones*, *directivas*, *tablas*) en el repositorio de Git. Copie los archivos de [aquí](https://github.com/Azure/azure-kusto-docs-samples/tree/master/DevOps_release_pipeline) en las respectivas carpetas como se muestra a continuación y confirmar los cambios. Se proporcionan los archivos de ejemplo para ejecutar el siguiente flujo de trabajo.

![Crear carpetas](media/devops/create-folders.png)

> [!TIP]
> Al crear su propio flujo de trabajo, se recomienda realizar su idempotente de código. Por ejemplo, usar [tabla .create mezcla](/azure/kusto/management/tables#create-merge-tables) en lugar de [.create tabla](/azure/kusto/management/tables#create-table)y usar [.create o alter](/azure/kusto/management/functions#create-or-alter-function) función en lugar de [.create](/azure/kusto/management/functions#create-function) función.

## <a name="create-a-release-pipeline"></a>Creación de una canalización de versión

1. Inicie sesión en su [organización de Azure DevOps](https://dev.azure.com/).
1. Seleccione **canalizaciones** > **versiones** desde el menú izquierdo y seleccione **nueva canalización**.

    ![Nueva canalización](media/devops/new-pipeline.png)

1. El **nueva canalización de versiones** abre la ventana. En el **canalizaciones** ficha la **seleccionar una plantilla** panel, seleccione **trabajos vacías**.

     ![Seleccione una plantilla:](media/devops/select-template.png)

1. Seleccione **fase** botón. En **fase** panel, agregue el **nombre de fase**. Seleccione **guardar** para guardar la canalización.

    ![Nombre de la fase](media/devops/stage-name.png)

1. Seleccione **agregar un artefacto** botón. En el **agregar un artefacto** panel, seleccione el repositorio donde existe su código, rellene la información pertinente y haga clic en **agregar**. Seleccione **guardar** para guardar la canalización.

    ![Agregar un artefacto](media/devops/add-artifact.png)

1. En el **Variables** ficha, seleccione **+ agregar** para crear una variable para **dirección URL del extremo** que se usará en la tarea. Escribir el **nombre** y **valor** del punto de conexión. Seleccione **guardar** para guardar la canalización. 

    ![Crear variable](media/devops/create-variable.png)

    Para buscar su Endpoint_URL, la página de información general de su **Cluster de Azure Data Explorer** en Azure portal contiene el URI de clúster de explorador de datos de Azure. Construir el URI en el siguiente formato `https://<Azure Data Explorer cluster URI>?DatabaseName=<DBName>`.  Por ejemplo: https://kustodocs.westus.kusto.windows.net?DatabaseName=SampleDB

    ![URI del clúster de explorador de datos Azure](media/devops/adx-cluster-uri.png)

## <a name="create-tasks-to-deploy"></a>Crear tareas para implementar

1. En el **canalización** pestaña, haga clic en **1 trabajo, tarea 0** para agregar tareas. 

    ![Adición de tareas](media/devops/add-task.png)

1. Cree tres tareas para implementar **tablas**, **funciones**, y **directivas**, en este orden. 

1. En el **tareas** ficha, seleccione **+** por **trabajo del agente**. Busque **Azure Data Explorer**. En **Marketplace**, instale el **Explorador de datos de Azure: comandos de administrador** extensión. A continuación, seleccione **agregar** en **ejecutar un comando de explorador de datos de Azure**.

     ![Agregar comandos de administrador](media/devops/add-admin-commands.png)

1. Haga clic en **Kusto comando** a la izquierda y la actualización de la tarea con la siguiente información:
    * **Nombre para mostrar**: Nombre de la tarea
    * **Ruta de acceso del archivo**: En el **tablas** de tareas, especifique *nombreDeLaTabla*.csl puesto que los archivos de creación de tabla están en el *tabla* carpeta.
    * **Dirección URL del extremo**: escriba el `EndPoint URL`variable creada en el paso anterior.
    * Seleccione **punto de conexión de servicio de uso** y seleccione **+ nuevo**.

    ![Actualizar la tarea de comando de Kusto](media/devops/kusto-command-task.png)

1. Complete la siguiente información en el **conexión de servicio de explorador de datos de Azure de agregar** ventana:

    |Configuración  |Valor sugerido  |
    |---------|---------|
    |**Nombre de la conexión**     |    Escriba un nombre para identificar este punto de conexión de servicio     |
    |**Dirección Url del clúster**    |    Valor puede encontrarse en la sección de información general del clúster de explorador de datos de Azure en Azure portal | 
    |**Id. de entidad de servicio**    |    Escriba el identificador de aplicación de AAD (creado como requisito previo)     |
    |**Clave de aplicación de la entidad de servicio**     |    Escriba la clave de aplicación de AAD (creado como requisito previo)    |
    |**Id. de inquilino AAD**    |      Escriba a su inquilino de AAD (por ejemplo, microsoft.com, contoso.com...)    |

    Seleccione **permitir todas las canalizaciones usar esta conexión** casilla de verificación. Seleccione **Aceptar**.

    ![Agregar conexión de servicio](media/devops/add-service-connection.png)

1. Repita los pasos del 1 al 5 otro dos veces para implementar los archivos desde el *funciones* y *directivas* carpetas. Seleccione **Guardar**. En el **tareas** pestaña, vea las tres tareas creadas: **Implementar tablas**, **implementar funciones**, y **implementar directivas**.

    ![Implementar todas las carpetas](media/devops/deploy-all-folders.png)

1. Seleccione **+ versión** > **crear versión** para crear una versión.

    ![Creación de una versión](media/devops/create-release.png)

1. En el **registros** ficha, compruebe el estado de implementación es correcta.

    ![La implementación es correcta](media/devops/deployment-successful.png)

Ahora ha completado la creación de una canalización de versiones para la implementación de tres tareas para preproducción.