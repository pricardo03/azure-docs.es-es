---
title: Integración del control de código fuente en Azure Automation
description: En este artículo se describe la integración del control de código fuente con GitHub en Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 81602f1a30fb753d7a8fcfccace581cd8c7b2f0c
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59607099"
---
# <a name="source-control-integration-in-azure-automation"></a>Integración del control de código fuente en Azure Automation

Control de código fuente le permite mantener sus runbooks en la automatización de la cuenta están actualizados con las secuencias de comandos en el repositorio de control de código fuente de GitHub o repositorios de Azure. El control de código fuente le permite colaborar fácilmente con su equipo, realizar el seguimiento de los cambios y revertir a versiones anteriores de los runbooks. Por ejemplo, el control de código fuente permite sincronizar distintas ramas de control de código fuente con las cuentas de Automation de desarrollo, prueba o producción. Esto facilita la promoción de código que se ha probado en el entorno de desarrollo a la cuenta de Automation de producción. Integración de control de código fuente con automation admite la sincronización de una dirección única desde el repositorio de control de código fuente.

Azure Automation admite tres tipos de control de código fuente:

* GitHub
* Repositorios de Azure (Git)
* Azure Repos (TFVC)

## <a name="pre-requisites"></a>Requisitos previos

* Un repositorio de control de código fuente (GitHub o repositorios de Azure)
* Un [cuenta de ejecución](manage-runas-account.md)
* Asegúrese de que tiene el [módulos de Azure más recientes](automation-update-azure-modules.md) en su cuenta de Automation

> [!NOTE]
> Los trabajos de sincronización de control de código fuente se ejecutan en la cuenta de Automation de los usuarios y se facturan con la misma tarifa que otros trabajos de Automation.

## <a name="configure-source-control---azure-portal"></a>Configurar control de código fuente - Azure portal

Dentro de su cuenta de Automation, seleccione **Control de código fuente** y haga clic en **+ agregar**

![Seleccionar control de código fuente](./media/source-control-integration/select-source-control.png)

Elija **Tipo de control de código fuente** y haga clic en **Autenticar**. Abre una ventana de explorador y le pedirá que inicie sesión, siga las indicaciones para completar la autenticación.

En la página **Resumen del control de código fuente**, rellene la información y haga clic en **Guardar**. En la tabla siguiente se proporciona breve descripción de los campos disponibles.

|Propiedad  |DESCRIPCIÓN  |
|---------|---------|
|Nombre del control de código fuente     | Un nombre descriptivo para el control de código fuente. *Este nombre debe contener solo letras y números.*        |
|Tipo de control de código fuente     | Tipo del control de código fuente. Las opciones disponibles son la siguientes:</br> GitHub</br>Repositorios de Azure (Git)</br> Azure Repos (TFVC)        |
|Repositorio     | Nombre del propietario del repositorio o proyecto. Se devuelven los 200 primeros repositorios. Para buscar un repositorio, escriba el nombre del campo y haga clic en **Search en GitHub**.|
|Rama     | Rama de la que se van a extraer los archivos de código fuente. Destino de bifurcación no está disponible para el tipo de control de código fuente TFVC.          |
|Ruta de acceso a la carpeta     | Carpeta que contiene los runbooks que se van a sincronizar. Ejemplo: /Runbooks </br>*Se sincronizan runbooks solo en la carpeta especificada. No se admite la recursión.*        |
|Sincronización automática     | Activa o desactiva la sincronización automática cuando se realiza una confirmación en el repositorio de control de código fuente.         |
|Publicar runbook     | Si establece en **en**, una vez que se sincronizan runbooks desde control de código fuente deberá publicarse automáticamente.         |
|DESCRIPCIÓN     | Campo de texto para proporcionar detalles adicionales.        |

![Resumen de control de código fuente](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Asegúrese de que ha iniciado sesión con la cuenta correcta al configurar el control de código fuente. Si tiene dudas, abra una nueva pestaña en el explorador, cierre la sesión de visualstudio.com o github.com y vuelva a intentar establecer la conexión con el control de código fuente.

## <a name="configure-source-control---powershell"></a>Configurar control de código fuente - PowerShell

También puede usar PowerShell para configurar el control de código fuente en Azure Automation. Para configurar el control de código fuente con los cmdlets de PowerShell, se necesita un token de acceso personal (PAT). Usa el [New AzureRmAutomationSourceControl](/powershell/module/AzureRM.Automation/New-AzureRmAutomationSourceControl) para crear la conexión de control de código fuente. El cmdlet requiere una cadena segura de los Token de acceso Personal, para aprender a crear una cadena segura, consulte [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

### <a name="azure-repos-git"></a>Repositorios de Azure (Git)

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

El control de código fuente requiere algunos permisos mínimos para los tokens de acceso personal. Las tablas siguientes contienen los permisos mínimos necesarios para GitHub y repositorios de Azure.

#### <a name="github"></a>GitHub

Para obtener más información sobre cómo crear un token de acceso personal en GitHub, visite [creación de un token de acceso personal para la línea de comandos](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

|Ámbito  |DESCRIPCIÓN  |
|---------|---------|
|**repo**     |         |
|repo:status     | Acceder al estado de confirmación         |
|repo_deployment      | Acceder al estado de implementación         |
|public_repo     | Acceder a los repositorios públicos         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Escribir los enlaces de repositorio         |
|read:repo_hook|Leer los enlaces de repositorio|

#### <a name="azure-repos"></a>Azure Repos

Para obtener más información sobre cómo crear un token de acceso personal en repositorios de Azure, visite [autenticar el acceso con tokens de acceso personal](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

|Ámbito  |
|---------|
|Código (leer)     |
|Proyecto y equipo (leer)|
|Identidad (leer)      |
|Perfil de usuario (leer)     |
|Elementos de trabajo (leer)    |
|Conexiones de servicio (leer, consultar y administrar)<sup>1</sup>    |

<sup>1</sup> permiso de las conexiones de servicio solo es necesario si ha habilitado la sincronización automática.

## <a name="syncing"></a>Sincronización

Seleccione el origen de la tabla en la **control de código fuente** página. Haga clic en **Iniciar sincronización** para iniciar el proceso de sincronización.

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

Inicio de sesión adicional está disponible si selecciona **todos los registros** en el **resumen del trabajo de sincronización de Control de código fuente** página. Estas entradas de registro adicionales que pueden ayudarle a solucionar los problemas que pueden surgir al usar el control de código fuente.

## <a name="disconnecting-source-control"></a>Desconexión del control de código fuente

Para desconectarse de un repositorio de control de código fuente, abra **control de código fuente** en **configuración de la cuenta** en su cuenta de Automation.

Seleccione el control de código fuente que quiera quitar. En la página **Resumen del control de código fuente**, haga clic en **Eliminar**.

## <a name="encoding"></a>Encoding

Si varias personas son Editar runbooks en el repositorio de control de código fuente con diferentes editores, hay una oportunidad de ejecutarse en problemas de codificación. Esta situación puede dar lugar a caracteres incorrectos en el runbook. Para obtener más información sobre esto, vea [las causas comunes de problemas de codificación](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los tipos de Runbook, sus ventajas y sus limitaciones, consulte [Tipos de runbooks de Azure Automation](automation-runbook-types.md)
