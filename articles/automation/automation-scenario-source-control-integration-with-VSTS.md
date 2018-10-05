---
title: Integración de Azure Automation con el control de código fuente de Azure DevOps Services
description: En este escenario se recorre la configuración de la integración con una cuenta de Azure Automation y el control de código fuente de Azure DevOps Services.
services: automation
author: eamonoreilly
ms.author: eamono
keywords: azure powershell, Azure DevOps Services, control de código fuente, automation
ms.service: automation
ms.component: process-automation
ms.topic: conceptual
ms.date: 03/19/2017
ms.openlocfilehash: ef21060e98972dd7bc561f9a4311fa0c4bdec3b4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227068"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Escenario de Azure Automation: integración del control del código fuente de Automation con Azure DevOps

En este escenario, tiene un proyecto de Azure DevOps que usa para administrar los runbooks de Azure Automation o las configuraciones de DSC bajo el control de código fuente.
En este artículo se describe cómo integrar Azure DevOps con el entorno de Azure Automation para que se produzca la integración continua en cada inserción.

## <a name="getting-the-scenario"></a>Obtención del escenario

Este escenario consta de dos Runbooks de PowerShell que puede importar directamente desde la [Galería de Runbooks](automation-runbook-gallery.md) en Azure Portal o descargar de la [Galería de PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | DESCRIPCIÓN| 
--------|------------|
Sync-VSTS | Importa runbooks o configuraciones de control de código fuente de Azure DevOps cuando se realiza una inserción en el repositorio. Si se ejecuta manualmente, importa y publica todos los runbooks o configuraciones en la cuenta de Automation.| 
Sync-VSTSGit | Importa runbooks o configuraciones de control de código fuente de Azure DevOps en Git cuando se realiza una inserción en el repositorio. Si se ejecuta manualmente, importa y publica todos los runbooks o configuraciones en la cuenta de Automation.|

### <a name="variables"></a>variables

Variable | DESCRIPCIÓN|
-----------|------------|
VSToken | Protege el recurso de variable que se crea y que contiene el token de acceso personal de Azure DevOps. Puede aprender a crear un token de acceso personal de Azure DevOps en la [página de autenticación de Azure DevOps](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Instalación y configuración de este escenario

Cree un [token de acceso personal](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) en Azure DevOps y úselo para sincronizar los runbooks o las configuraciones en su cuenta de Automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Cree una [variable segura](automation-variables.md) en su cuenta de Automation que contenga el token de acceso personal para que el runbook se pueda autenticar en Azure DevOps y sincronice los runbooks o las configuraciones en la cuenta de Automation. Puede llamarla VSToken.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importe el Runbook que sincroniza los runbooks o las configuraciones en la cuenta de automatización. Puede usar el [runbook de ejemplo de Azure DevOps](https://www.powershellgallery.com/packages/Sync-VSTS) o el [runbook de ejemplo de Azure DevOps con GIT](https://www.powershellgallery.com/packages/Sync-VSTSGit) desde la [Galería de PowerShell](https://www.powershellgallery.com) en función de que use el control de código fuente de Azure DevOps o Azure DevOps con GIT e implementarlo en su cuenta de Automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Ahora puede [publicar](automation-creating-importing-runbook.md#publishing-a-runbook) este runbook y así crear un webhook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Cree un [webhook](automation-webhooks.md) para este runbook Sync-VSTS y rellene los parámetros como se muestra a continuación. Asegúrese de que copia la dirección URL del webhook, ya que la necesita para un enlace de servicio en Azure DevOps. VSAccessTokenVariableName es el nombre (VSToken) de la variable segura que creó anteriormente para contener el token de acceso personal. 

La integración con Azure DevOps (Sync-VSTS.ps1) usa los siguientes parámetros:
### <a name="sync-vsts-parameters"></a>Parámetros de Sync-VSTS

Parámetro | DESCRIPCIÓN| 
--------|------------|
WebhookData | Contiene la información de inserción en el repositorio enviada desde el enlace de servicio de Azure DevOps. Este parámetro se debe dejar en blanco.| 
ResourceGroup | Este es el nombre del grupo de recursos en el que se encuentra la cuenta de automatización.|
AutomationAccountName | El nombre de la cuenta de Automation que se sincroniza con Azure DevOps.|
VSFolder | El nombre de la carpeta de Azure DevOps en la que existen los runbooks y las configuraciones.|
VSAccount | El nombre de la organización de Azure DevOps.| 
VSAccessTokenVariableName | El nombre de la variable segura (VSToken) que contiene el token de acceso personal de Azure DevOps.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Si usa Azure DevOps con GIT (Sync-VSTSGit.ps1), tomará los siguientes parámetros.

Parámetro | DESCRIPCIÓN|
--------|------------|
WebhookData | Contendrá la información de inserción en el repositorio enviada desde el enlace de servicio de Azure DevOps. Este parámetro se debe dejar en blanco.| 
ResourceGroup | Este es el nombre del grupo de recursos en el que se encuentra la cuenta de automatización.|
AutomationAccountName | El nombre de la cuenta de Automation que se sincroniza con Azure DevOps.|
VSAccount | El nombre de la organización de Azure DevOps.|
VSProject | El nombre del proyecto de Azure DevOps en el que existen los runbooks y las configuraciones.|
GitRepo | El nombre del repositorio GIT.|
GitBranch | El nombre de la rama del repositorio GIT de Azure DevOps.|
Carpeta | El nombre de la carpeta en la rama de GIT de Azure DevOps.|
VSAccessTokenVariableName | El nombre de la variable segura (VSToken) que contiene el token de acceso personal de Azure DevOps.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Cree un enlace de servicio en Azure DevOps para las inserciones en el repositorio en la carpeta que desencadena este webhook en la inserción de código en el repositorio. Seleccione **Enlaces web** como el servicio con el que realizar la integración al crear una nueva suscripción. Puede obtener más información acerca de los enlaces de servicio en la [documentación de los enlaces de servicio de Azure DevOps](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Ahora debería poder realizar todas las inserciones en el repositorio de sus runbooks y configuraciones en Azure DevOps, así como sincronizarlos automáticamente en su cuenta de Automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Si ejecuta este Runbook manualmente sin que lo desencadene Azure DevOps, puede dejar vacío el parámetro webhookdata y realiza una sincronización completa desde la carpeta de Azure DevOps especificada.

Si desea desinstalar el escenario, quite el enlace del servicio de Azure DevOps y elimine el runbook y la variable VSToken.
