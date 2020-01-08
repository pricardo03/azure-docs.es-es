---
title: Integración del control de código fuente en Azure Automation
description: En este artículo se describe la integración del control de código fuente con GitHub en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 837ebd71886e9435a44080b06c079623c3936c69
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75417064"
---
# <a name="source-control-integration-in-azure-automation"></a>Integración del control de código fuente en Azure Automation

El control de código fuente permite mantener actualizados los runbooks de la cuenta de Automation con los scripts del repositorio de control de código fuente de GitHub o de Azure Repos. El control de código fuente le permite colaborar fácilmente con su equipo, realizar el seguimiento de los cambios y revertir a versiones anteriores de los runbooks. Por ejemplo, el control de código fuente permite sincronizar distintas ramas de control de código fuente con las cuentas de Automation de desarrollo, prueba o producción. Esto facilita la promoción de código que se ha probado en el entorno de desarrollo a la cuenta de Automation de producción. La integración del control de código fuente con la automatización permite la sincronización en una sola dirección desde el repositorio de control de código fuente.

Azure Automation admite tres tipos de control de código fuente:

* GitHub
* Azure Repos (Git)
* Azure Repos (TFVC)

## <a name="pre-requisites"></a>Requisitos previos

* Un repositorio de control de código fuente (GitHub o Azure Repos).
* Una [cuenta de ejecución](manage-runas-account.md).
* Asegúrese de tener los [módulos de Azure más recientes](automation-update-azure-modules.md) en su cuenta de Automation, incluido el módulo **AzureRM.Profile**. 

> [!NOTE]
> Los trabajos de sincronización de control de código fuente se ejecutan en la cuenta de Automation de los usuarios y se facturan con la misma tarifa que otros trabajos de Automation.

## <a name="configure-source-control---azure-portal"></a>Configuración del control de código fuente: Azure Portal

Dentro de su cuenta de Automation, seleccione **Control de código fuente** y haga clic en **+ Agregar**.

![Seleccionar control de código fuente](./media/source-control-integration/select-source-control.png)

Elija **Tipo de control de código fuente** y haga clic en **Autenticar**. Se abre una ventana de explorador que le pide que inicie sesión; siga las indicaciones para completar la autenticación.

En la página **Resumen del control de código fuente**, rellene la información y haga clic en **Guardar**. En la tabla siguiente se proporciona breve descripción de los campos disponibles.

|Propiedad  |Descripción  |
|---------|---------|
|Nombre del control de código fuente     | Nombre descriptivo del control de código fuente. *Este nombre solo puede contener letras y números.*        |
|Tipo de control de código fuente     | Tipo del control de código fuente. Las opciones disponibles son la siguientes:</br> GitHub</br>Azure Repos (Git)</br> Azure Repos (TFVC)        |
|Repositorio     | Nombre del propietario del repositorio o proyecto. Se devuelven los 200 primeros repositorios. Para buscar un repositorio, escriba el nombre en el campo y haga clic en **Search on GitHub** (Buscar en GitHub).|
|Rama     | Rama de la que se van a extraer los archivos de código fuente. No se permite especificar una rama para el tipo de control de código fuente TFVC.          |
|Ruta de acceso a la carpeta     | Carpeta que contiene los runbooks que se van a sincronizar. Ejemplo: /Runbooks </br>*Solo se sincronizan los de la carpeta especificada. No se admite la recursión.*        |
|Sincronización automática<sup>1</sup>     | Activa o desactiva la sincronización automática cuando se realiza una confirmación en el repositorio de control de código fuente.         |
|Publicar runbook     | Si se establece en **Activado**, una vez que los runbooks se sincronizan desde el control de código fuente, se publican automáticamente.         |
|Descripción     | Campo de texto para proporcionar detalles adicionales.        |

<sup>1</sup> Para habilitar la sincronización automática al configurar la integración del control de código fuente con Azure Repos, debe ser un administrador del proyecto.

![Resumen de control de código fuente](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> El inicio de sesión en el repositorio del control de código fuente puede ser diferente al inicio de sesión en Azure Portal. Al configurar el control de código fuente, asegúrese de que ha iniciado sesión con la cuenta correcta en el repositorio del control de código fuente. Si tiene dudas, abra una nueva pestaña en el explorador, cierre la sesión de visualstudio.com o github.com y vuelva a intentar establecer la conexión con el control de código fuente.

## <a name="configure-source-control---powershell"></a>Configuración del control de código fuente: PowerShell

También puede usar PowerShell para configurar el control de código fuente en Azure Automation. Para configurar el control de código fuente con los cmdlets de PowerShell, se necesita un token de acceso personal (PAT). Puede usar [New-AzureRmAutomationSourceControl](/powershell/module/AzureRM.Automation/New-AzureRmAutomationSourceControl) para crear la conexión al control de código fuente. El cmdlet requiere una cadena segura del token de acceso personal. Para aprender a crear una, consulte [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

### <a name="azure-repos-git"></a>Azure Repos (Git)

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCReposGit -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

### <a name="azure-repos-tfvc"></a>Azure Repos (TFVC)

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCReposTFVC -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_versionControl -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

### <a name="github"></a>GitHub

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="personal-access-token-permissions"></a>Permisos de token de acceso personal

El control de código fuente requiere algunos permisos mínimos para los tokens de acceso personal. Las tablas siguientes contienen los permisos mínimos necesarios para GitHub y Azure Repos.

#### <a name="github"></a>GitHub

Para más información sobre cómo crear un token de acceso personal en GitHub, visite [Creating a personal access token for the command line](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) (Creación de un token de acceso personal para la línea de comandos).

|Ámbito  |Descripción  |
|---------|---------|
|**repo**     |         |
|repo:status     | Acceder al estado de confirmación         |
|repo_deployment      | Acceder al estado de implementación         |
|public_repo     | Acceder a los repositorios públicos         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Escribir los enlaces de repositorio         |
|read:repo_hook|Leer los enlaces de repositorio|

#### <a name="azure-repos"></a>Azure Repos

Para más información sobre cómo crear un token de acceso personal en Azure Repos, visite [Authenticate access with personal access tokens](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) (Autenticación del acceso con tokens de acceso personal).

|Ámbito  |
|---------|
|Código (leer)     |
|Proyecto y equipo (leer)|
|Identidad (leer)      |
|Perfil de usuario (leer)     |
|Elementos de trabajo (leer)    |
|Conexiones de servicio (leer, consultar y administrar)<sup>1</sup>    |

<sup>1</sup>El permiso de Conexiones de servicio solo es necesario si ha habilitado la sincronización automática.

## <a name="syncing"></a>Sincronización

Seleccione el origen de la tabla en la página **Control de código fuente**. Haga clic en **Iniciar sincronización** para iniciar el proceso de sincronización.

Para ver el estado de la tarea de sincronización actual o de las anteriores, haga clic en la pestaña **Trabajos de sincronización**. En la lista desplegable **Control de código fuente**, seleccione un control de código fuente.

![Estado de sincronización](./media/source-control-integration/sync-status.png)

Al hacer clic en un trabajo, verá la salida del trabajo. El siguiente es un ejemplo de la salida de un trabajo de sincronización de control de código fuente.

```output
========================================================================================================

Azure Automation Source Control.
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

Puede encontrar opciones de registro adicionales si selecciona **Todos los registros** en la página **Source Control Sync Job Summary** (Resumen del trabajo de sincronización del control de código fuente). Estas entradas de registro adicionales pueden ayudarlo a solucionar los problemas que pueden surgir al usar el control de código fuente.

## <a name="disconnecting-source-control"></a>Desconexión del control de código fuente

Para desconectarse de un repositorio de control de código fuente, abra **Control de código fuente** en **Configuración de la cuenta**, en su cuenta de Automation.

Seleccione el control de código fuente que quiera quitar. En la página **Resumen del control de código fuente**, haga clic en **Eliminar**.

## <a name="encoding"></a>Encoding

Si varias personas editan runbooks en el repositorio de control de código fuente con diferentes editores, existe la probabilidad de encontrarse con problemas de codificación. Esta situación puede dar lugar a caracteres incorrectos en el runbook. Para más información al respecto, consulte [Causas comunes de problemas de codificación](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues).

## <a name="updating-the-access-token"></a>Actualización del token de acceso

Actualmente no hay ninguna manera de actualizar el token de acceso en el control de código fuente desde el portal. Una vez que el token de acceso personal ha expirado o se ha revocado, puede actualizar el control de código fuente con un nuevo token de acceso de las siguientes maneras:

* A través de la [API REST](https://docs.microsoft.com/rest/api/automation/sourcecontrol/update).
* Mediante el cmdlet [Update-AzAutomationSourceControl](/powershell/module/az.automation/update-azautomationsourcecontrol).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los tipos de Runbook, sus ventajas y sus limitaciones, consulte [Tipos de runbooks de Azure Automation](automation-runbook-types.md)
