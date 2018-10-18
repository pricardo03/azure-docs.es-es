---
title: Administrar paquetes de Python 2 en Azure Automation
description: En este artículo se describe cómo administrar paquetes de Python 2 en Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5974a8e622ca0969b2a7b5ee9500766ac95398c9
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45987226"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Administrar paquetes de Python 2 en Azure Automation

Azure Automation le permite ejecutar runbooks de Python 2 en Azure y en Hybrid Runbook Worker de Linux. Para simplificar los runbooks, puede usar los paquetes de Python para importar los módulos que necesite. En este artículo se describe cómo administrar y usar paquetes de Python en Azure Automation.

## <a name="import-packages"></a>Importar paquetes

En la cuenta de Automation, seleccione **Python 2 packages** (Paquetes de Python 2) en **Recursos compartidos**. Haga clic en **+ Add a Python 2 package** (+ Agregar un paquete de Python 2).

![Agregar un paquete de Python](media/python-packages/add-python-package.png)

En la página **Agregar un paquete de Python 2**, seleccione un paquete local para cargar. El paquete puede ser un archivo `.whl` o `.tar.gz`. Cuando seleccione esta opción, haga clic en **Aceptar** para cargar el paquete.

![Agregar un paquete de Python](media/python-packages/upload-package.png)

Una vez que se haya importado un paquete, se muestra en la página de **paquetes de Python 2** en su cuenta de Automation. Si necesita quitar un paquete, seleccione el paquete y elija **Eliminar** en la página del paquete.

![Lista de paquetes](media/python-packages/package-list.png)

## <a name="use-a-package-in-a-runbook"></a>Usar un paquete en un runbook

Después de importar un paquete,se puede usar en un runbook. En el ejemplo siguiente se usa el [ paquete de utilidades de Azure Automation](https://github.com/azureautomation/azure_automation_utility). Este paquete facilita el uso de Python con Azure Automation. Para usar el paquete, siga las instrucciones del repositorio de GitHub y agréguelo al runbook mediante `from azure_automation_utility import get_automation_runas_credential`; por ejemplo, para importar la función para recuperar la cuenta de tipo RunAs.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Desarrollar y probar runbooks sin conexión

Para desarrollar y probar los runbooks de Python 2 sin conexión, puede usar el módulo [Azure Automation python emulated assets](https://github.com/azureautomation/python_emulated_assets) (Recursos emulados de Python de Azure Automation) en GitHub. Este módulo le permite hacer referencia a recursos compartidos como credenciales, variables, conexiones y certificados.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con runbooks de Python 2, consulte [My first Python 2 runbook](automation-first-runbook-textual-python2.md) (Mi primer runbook de Python 2).