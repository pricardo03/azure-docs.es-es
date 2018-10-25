---
title: Integración del control de código fuente en Azure Automation
description: En este artículo se describe la integración del control de código fuente con GitHub en Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/26/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9bbf3582da2664b6e6429677d47aad4d69a7c1bb
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785331"
---
# <a name="source-control-integration-in-azure-automation"></a>Integración del control de código fuente en Azure Automation

El control de código fuente permite mantener los runbooks de su cuenta de Automation actualizados con los scripts de su repositorio de control de código fuente de GitHub o de Azure DevOps. El control de código fuente le permite colaborar fácilmente con su equipo, realizar el seguimiento de los cambios y revertir a versiones anteriores de los runbooks. Por ejemplo, el control de código fuente permite sincronizar distintas ramas de control de código fuente con las cuentas de Automation de desarrollo, prueba o producción. Esto facilita la promoción de código que se ha probado en el entorno de desarrollo a la cuenta de Automation de producción.

Azure Automation admite 3 tipos de control de código fuente:

* GitHub
* Visual Studio Team Services (Git)
* Visual Studio Team Services (TFVC)

## <a name="pre-requisites"></a>Requisitos previos

* Un repositorio de control de código fuente (GitHub o Visual Studio Team Services)
* Los [permisos](#personal-access-token-permissions) correctos en el repositorio de control de código fuente
* Una [cuenta de ejecución y conexión](manage-runas-account.md)

> [!NOTE]
> Los trabajos de sincronización de control de código fuente se ejecutan en la cuenta de Automation de los usuarios y se facturan con la misma tarifa que otros trabajos de Automation.

## <a name="configure-source-control"></a>Configurar el control de código fuente

Dentro de su cuenta de Automation, seleccione **Control de código fuente (versión preliminar)** y haga clic en **+ Agregar**.

![Seleccionar control de código fuente](./media/source-control-integration/select-source-control.png)

Elija **Tipo de control de código fuente** y haga clic en **Autenticar**.

Revise la página de permisos de solicitud de la aplicación y haga clic en **Aceptar**.

En la página **Resumen del control de código fuente**, rellene la información y haga clic en **Guardar**. En la tabla siguiente se proporciona breve descripción de los campos disponibles.

|Propiedad  |DESCRIPCIÓN  |
|---------|---------|
|Nombre del control de código fuente     | Nombre descriptivo del control de código fuente.        |
|Tipo de control de código fuente     | Tipo del control de código fuente. Las opciones disponibles son la siguientes:</br> Github</br>Visual Studio Team Services (Git)</br> Visual Studio Team Services (TFVC)        |
|Repositorio     | Nombre del propietario del repositorio o proyecto. Este valor se extrae del repositorio de control de código fuente. Ejemplo: $/ContosoFinanceTFVCExample         |
|Rama     | Rama de la que se van a extraer los archivos de código fuente. No se permite especificar una rama para el tipo de control de código fuente TFVC.          |
|Ruta de acceso a la carpeta     | Carpeta que contiene los runbooks que se van a sincronizar. Ejemplo: /Runbooks         |
|Sincronización automática     | Activa o desactiva la sincronización automática cuando se realiza una confirmación en el repositorio de control de código fuente.         |
|Publicar runbook     | Si establece en **Activado**, una vez que están sincronizados con el control de código fuente, se publican automáticamente.         |
|DESCRIPCIÓN     | Campo de texto para proporcionar detalles adicionales.        |

![Resumen de control de código fuente](./media/source-control-integration/source-control-summary.png)

## <a name="syncing"></a>Sincronización

Si se estableció la sincronización automática al configurar la integración del control de código fuente, la sincronización inicial se iniciará automáticamente. Si no se estableció la sincronización automática, seleccione el origen en la tabla de la página **Control de código fuente (versión preliminar)**. Haga clic en **Iniciar sincronización** para iniciar el proceso de sincronización.  

Para ver el estado de la tarea de sincronización actual o de las anteriores, haga clic en la pestaña **Trabajos de sincronización**. En la lista desplegable **Control de código fuente**, seleccione un control de código fuente.

![Estado de sincronización](./media/source-control-integration/sync-status.png)

Al hacer clic en un trabajo, verá la salida del trabajo. El siguiente es un ejemplo de la salida de un trabajo de sincronización de control de código fuente.

```output
========================================================================================================

Azure Automation Source Control Public Preview.
Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

Setting AzureRmEnvironment.

Getting AzureRunAsConnection.

Logging in to Azure...

Source control information for syncing:

[Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

Connecting to VSTS...


Source Control Sync Summary:


2 files synced:
 - ExampleRunbook1.ps1
 - ExampleRunbook2.ps1



========================================================================================================
```

## <a name="personal-access-token-permissions"></a>Permisos de token de acceso personal

El control de código fuente requiere algunos permisos mínimos para los tokens de acceso personal. Las tablas siguientes contienen los permisos mínimos necesarios para GitHub y Azure DevOps.

### <a name="github"></a>GitHub

|Ámbito  |DESCRIPCIÓN  |
|---------|---------|
|**repo**     |         |
|repo:status     | Acceder al estado de confirmación         |
|repo_deployment      | Acceder al estado de implementación         |
|public_repo     | Acceder a los repositorios públicos         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Escribir los enlaces de repositorio         |
|read:repo_hook|Leer los enlaces de repositorio|

### <a name="azure-devops"></a>Azure DevOps

|Ámbito  |
|---------|
|Código (leer)     |
|Proyecto y equipo (leer)|
|Identidad (leer)      |
|Perfil de usuario (leer)     |
|Elementos de trabajo (leer)    |
|Conexiones de servicio (leer, consultar y administrar)<sup>1</sup>    |

<sup>1</sup>El permiso de Conexiones de servicio solo es necesario si ha habilitado la sincronización automática.

## <a name="disconnecting-source-control"></a>Desconexión del control de código fuente

Para desconectarse de un repositorio de control de código fuente, abra **Control de código fuente (versión preliminar)** en **Configuración de la cuenta**, en su cuenta de Automation.

Seleccione el control de código fuente que quiera quitar. En la página **Resumen del control de código fuente**, haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los tipos de Runbook, sus ventajas y sus limitaciones, consulte [Tipos de runbooks de Azure Automation](automation-runbook-types.md)
